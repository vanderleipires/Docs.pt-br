---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Pré-compilação seu site (c#) | Microsoft Docs
author: rick-anderson
description: 'O Visual Studio oferece aos desenvolvedores ASP.NET dois tipos de projetos: Web Application Projects (WAPs) e projetos de Site (WSPs). Um dos betwe principais diferenças...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: af11b8f13979eb4613195d1fa30f2c1adf508187
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823510"
---
<a name="precompiling-your-website-c"></a>Pré-compilação seu site (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> O Visual Studio oferece aos desenvolvedores ASP.NET dois tipos de projetos: Web Application Projects (WAPs) e projetos de Site (WSPs). Uma das principais diferenças entre os dois tipos de projeto é que WAPs devem ter o código compilado explicitamente antes da implantação, enquanto o código em um WSP pode ser compilado automaticamente no servidor web. No entanto, é possível pré-compilar um WSP antes da implantação. Este tutorial explora os benefícios de pré-compilação e mostra como pré-compilar um site de dentro do Visual Studio e da linha de comando.


## <a name="introduction"></a>Introdução

O Visual Studio oferece aos desenvolvedores ASP.NET dois diferentes tipos de projeto: projetos de aplicativos Web (WAP) e projetos de Site da Web (WSP). Uma das principais diferenças entre esses tipos de projeto é que exigem que WAPs *compilação explícita* enquanto usam WSPs *compilação automática*, por padrão. Com WAPs, você compila código do aplicativo web em um único assembly, que é criado em um site do `Bin` pasta. Implantação envolve copiar o conteúdo de marcação (a `.aspx.ascx`, e `.master` arquivos) no projeto, juntamente com o assembly no `Bin` pasta; o code-behind arquivos de classe em si não precisa ser implantado. Por outro lado, você deve implantar WSPs, copiando as páginas de marcação e suas classes code-behind correspondente para o ambiente de produção. As classes code-behind são compiladas sob demanda no servidor web.

> [!NOTE]
> Consulte a seção "Explícita compilação em comparação com compilação automática" a [ *determinando quais arquivos precisam ser implantados* tutorial](determining-what-files-need-to-be-deployed-cs.md) para obter mais informações sobre as diferenças entre o projeto modelos de compilação explícita e automática e como o modelo de compilação afeta a implantação.


A opção de compilação automática é simple de usar. Não há nenhuma etapa de compilação explícita e somente os arquivos que foram modificados precisam ser implantados, enquanto a compilação explícita exige a implantação de páginas alteradas de marcação e o assembly compilado por just. No entanto, a implantação automática tem duas desvantagens:

- Porque as páginas devem ser compiladas automaticamente quando elas forem acessadas pela primeira vez, pode haver um atraso curto, mas perceptível quando uma página ASP.NET é solicitada pela primeira vez depois de ser implantado.
- Compilação automática requer que os dois o declarativo marcação e código-fonte esteja presente no servidor web. Isso pode ser uma opção pouco atraente se você pretende vender o aplicativo web para clientes que irá instalá-lo em seus servidores web.

Se tanto dos dois acima limitações são separadores de palavras do negócio, você pode alterar para o modelo WAP pode ou *pré-compilar* WSP antes da implantação. Este tutorial examina as opções de pré-compilação mais adequadas para um site hospedado e orienta o processo de pré-compilação e a implantação de um site pré-compilado.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Uma visão geral de geração de código do ASP.NET e compilação

Antes de examinarmos as opções de pré-compilação disponíveis, vamos primeiro falar sobre a geração de código e compilação que ocorre quando uma página ASP.NET é solicitada pela primeira vez, pois ele foi criado ou atualizado pela última vez. Como você sabe, as páginas ASP.NET são compostos por duas partes: marcação declarativa na `.aspx` arquivo; e uma parte do código fonte, normalmente em um arquivo de classe code-behind separado (`.aspx.cs`). As etapas executadas pelo tempo de execução quando uma página ASP.NET é solicitada depende do modelo de compilação do aplicativo.

Com WAPs, o código-fonte de páginas deve ser compilado explicitamente em um único assembly antes de ser implantado. Durante a implantação, esse assembly e as várias páginas de marcação são copiadas para o ambiente de produção. Quando chega uma solicitação para o servidor web para uma página ASP.NET, o tempo de execução cria uma instância da classe de code-behind da página e chama seu `ProcessRequest` método, que inicia o ciclo de vida da página e, em última análise, gera o conteúdo da página, que é retornado para o solicitante. O tempo de execução pode trabalhar com a classe de code-behind da página ASP.NET, porque a classe code-behind já foi compilada em um assembly antes da implantação.

Com WSPs e compilação automática, não há nenhuma etapa de compilação explícita antes da implantação. Em vez disso, a implantação envolve copiar declarativa e o conteúdo do código de origem para o ambiente de produção. Quando chega uma solicitação para o servidor web para uma página ASP.NET pela primeira vez, pois a página foi criada ou atualizado pela última vez, o tempo de execução deve primeiro compilar a classe code-behind em um assembly. Esse assembly compilado é salvo na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, embora o local dessa pasta pode ser personalizado por meio de `<compilation tempDirectory="" />` elemento de `<system.web>`, geralmente no `Web.config`. Porque o assembly é salvo em disco, ele não precisa ser recompilado em solicitações subsequentes para a mesma página.

> [!NOTE]
> Como você esperaria, há um pequeno atraso ao solicitar uma página pela primeira vez (ou pela primeira vez, pois ele é alterado) em um site que usa a compilação automática conforme ele demora algum tempo para o servidor compilar o código da página e salvar o assembly resultante disco.


Em resumo, com a compilação explícita você é necessárias para compilar código-fonte do site antes da implantação, evitando que o tempo de execução realizar essa etapa. Com a compilação automática o tempo de execução lida com a compilação do código-fonte das páginas, mas com um custo de inicialização de pequena para a primeira visita a página, pois ele foi criado ou atualizado pela última vez.

Mas e quanto a parte declarativa de páginas ASP.NET (o `.aspx` arquivo)? É óbvio que há uma relação entre o `.aspx` arquivos e o código em suas classes code-behind, como definidos na marcação declarativa de controles da Web estão acessíveis no código. Também é óbvio que o conteúdo a `.aspx` arquivos influencia bastante a marcação renderizada gerada pela página. Então, como funciona o tempo de execução com o texto, HTML, e a sintaxe de controle de Web definidos na `.aspx` conteúdo processada do arquivo para gerar a página solicitada?

Não quero me desviar muito sobre os detalhes de implementação de nível inferior, que variam entre WAPs e WSPs, mas, resumindo o tempo de execução gera automaticamente um arquivo de classe que contém vários controles da Web como métodos e membros protegidos. Esse arquivo gerado é implementado como um *classe parcial* para a classe code-behind correspondente. ([Classes parciais](http://www.dotnet-guide.com/partialclasses.html) permitir para o conteúdo de uma única classe sejam distribuídas em vários arquivos.) Portanto, a classe code-behind é definida em dois lugares: no `.aspx.cs` arquivo que você crie e nessa classe gerada automaticamente criada pelo tempo de execução. Essa classe gerada automaticamente é armazenado no `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` pasta.

O importante retirar aqui é que, para uma página ASP.NET ser processado pelo tempo de execução ambas as suas declarativa e partes do código de origem devem ser compiladas em um assembly. Com WAPs, explicitamente, o código-fonte é compilado em um assembly antes da implantação, mas a marcação declarativa ainda deve ser convertida em código e compilada pelo tempo de execução no servidor web. Com WSPs usando compilação automática, o código-fonte e a marcação declarativa precisam ser compilado pelo servidor web.

É possível usar a compilação explícita com o modelo WSP. Compilar explicitamente a parte do código fonte, como com o modelo WAP. Além disso, você também pode compilar a marcação declarativa.

## <a name="precompilation-options"></a>Opções de pré-compilação

O .NET Framework vem com um [ferramenta de compilação do ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) que permite que você compilar o código-fonte (e até mesmo o conteúdo) de um aplicativo ASP.NET criado usando o modelo de WSP. Essa ferramenta foi lançada com o .NET Framework versão 2.0 e está localizada no `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` pasta; ele pode ser usado na linha de comando ou inicializado de dentro do Visual Studio por meio da opção de Publicar Site do menu Build.

A ferramenta de compilação oferece dois formulários gerais da compilação: in-loco pré-compilação e pré-compilação para implantação. Com pré-compilação no local que você executar o `aspnet_compiler.exe` ferramenta da linha de comando e especifique o caminho para o diretório virtual ou o caminho físico de um site que reside em seu computador. A ferramenta de compilação, em seguida, compila cada página ASP.NET no projeto, armazenar a versão compilada no `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` pasta exatamente como se as páginas de cada um foi visitadas pela primeira vez em um navegador. Pré-compilação no local pode acelerar a primeira solicitação feita ao recém-implantados páginas ASP.NET em seu site, porque reduz o tempo de execução da necessidade de executar esta etapa. No entanto, pré-compilação no local não é útil para a maioria dos sites hospedados porque ele requer que você seja capaz de executar programas a partir do servidor web de linha de comando. Esse nível de acesso não é permitida em ambientes de hospedagem compartilhadas.

> [!NOTE]
> Para obter mais informações sobre a pré-compilação in-loco, fazer check-out [How To: Precompile ASP.NET Web Sites](https://msdn.microsoft.com/library/ms227972.aspx) e [pré-compilação do ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Em vez de compilar as páginas no site da Web para o `Temporary ASP.NET Files` pasta pré-compilação para implantação compila as páginas em um diretório de sua escolha e, em um formato que pode ser implantado no ambiente de produção.

Existem dois tipos de pré-compilação para implantação que exploramos neste tutorial: pré-compilação com uma interface de usuário atualizável e pré-compilação com uma interface do usuário não atualizável. Pré-compilação com uma interface de usuário atualizável deixa a marcação declarativa na `.aspx`, `.ascx`, e `.master` arquivos, permitindo assim que um desenvolvedor exibir e, se desejado, modificar a marcação declarativa no servidor de produção. Pré-compilação com uma interface do usuário não atualizável gera `.aspx` páginas que são nulos de qualquer conteúdo e remove `.ascx` e `.master` arquivos, assim, ocultando a marcação declarativa e proibindo um desenvolvedor de alterando-o da ambiente de produção.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Pré-compilação para implantação com uma Interface de usuário atualizável

É a melhor maneira de entender a pré-compilação para implantação ver um exemplo em ação. Vamos pré-compilar o WSP de revisões de livro para implantação usando uma interface de usuário atualizável. A ferramenta de compilação do ASP.NET pode ser invocada no menu de compilação do Visual Studio ou na linha de comando. Esta seção examina usando a ferramenta de dentro do Visual Studio; a seção "pré-compilação da linha de comando" examina executando a ferramenta do compilador da linha de comando.

Abra o livro revisão WSP no Visual Studio, vá para o menu de compilação e selecione a opção de menu Publish Web Site. Isso inicia a caixa de diálogo Publish Web Site (consulte **Figura 1**), onde você pode especificar o local de destino, ou não é atualizável a interface do usuário do site pré-compilado e outras opções de ferramenta do compilador. O local de destino pode ser um servidor web remoto ou o servidor FTP, mas por enquanto, escolha uma pasta no disco rígido do computador. Como queremos pré-compilar o site com uma interface de usuário atualizável, deixe a caixa de seleção "Permitir que este site pré-compilado seja atualizado" marcada e clique Okey.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Figura 1**: A ferramenta de compilação do ASP.NET será pré-compilar o site para o local de destino especificado  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> A opção de Publicar Site no menu de compilação não está disponível no Visual Web Developer. Se você estiver usando o Visual Web Developer, você precisará usar a versão de linha de comando da ferramenta de compilação do ASP.NET, que é abordada na seção "pré-compilação da linha de comando".


Depois de pré-compilar o site, navegue até o local de destino que você inseriu na caixa de diálogo Publish Web Site. Reserve um tempo para comparar o conteúdo dessa pasta com o conteúdo de seu site. **Figura 2** mostra a pasta do site resenhas de livros. Observe que ele contém ambos `.aspx` e `.aspx.cs` arquivos. Além disso, observe que o `Bin` diretório inclui apenas um arquivo, `Elmah.dll`, que adicionamos o [tutorial anterior](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Figura 2**: contém o diretório do projeto `.aspx` e `.aspx.cs` arquivos; o `Bin` pasta inclui apenas `Elmah.dll`  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-cs/_static/image6.png))

**Figura 3** mostra a pasta do local de destino cujo conteúdo foram criado pela ferramenta de compilação do ASP.NET. Esta pasta não contém quaisquer arquivos code-behind. Além disso, esta pasta `Bin` diretório inclui vários assemblies e dois `.compiled` arquivos além do `Elmah.dll` assembly.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Figura 3**: A pasta do local de destino inclui os arquivos para a implantação  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-cs/_static/image9.png))

Ao contrário de compilação explícita no WAPs, a pré-compilação para o processo de implantação não cria um assembly para todo o site. Em vez disso, ele processa em lotes juntos várias páginas em cada assembly. Ele também compila a `Global.asax` arquivo (se houver) em seu próprio assembly, bem como quaisquer classes contidas no `App_Code` pasta. Os arquivos que contêm a marcação declarativa para ASP.NET web páginas, controles de usuário e as páginas mestras (`.aspx`, `.ascx`, e `.master` arquivos, respectivamente) são copiados como-está no diretório local de destino. Da mesma forma, o `Web.config` arquivo é copiado diretamente, juntamente com todos os arquivos estáticos, como imagens, as classes CSS e arquivos PDF. Para obter uma descrição mais formal de como a ferramenta de compilação lida com vários tipos de arquivo, consulte [tratamento de arquivo durante o pré-compilação ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Você pode instruir a ferramenta de compilação para criar um assembly por página ASP.NET, controle de usuário ou página mestra, marcando a caixa de seleção "Usado fixed naming and assemblies de página única" na caixa de diálogo Publish Web Site. Ter cada página do ASP.NET compilada em seu próprio assembly permite controle mais refinado sobre a implantação. Por exemplo, se você atualizou uma única página da web ASP.NET e necessários para implantar essa alteração, você só precisa implantar nessa página `.aspx` arquivo e assembly associado ao ambiente de produção. Consultar [How To: gerar nomes fixos com a ferramenta de compilação do ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) para obter mais informações.


O diretório local de destino também contém um arquivo que não fazia parte do projeto da web pré-compilado, ou seja, `PrecompiledApp.config`. Esse arquivo informa o tempo de execução do ASP.NET que o aplicativo estava pré-compilado e se ele estava pré-compilado com uma interface do usuário atualizável ou meio-dia atualizável.

Por fim, reserve um tempo para abrir um do `.aspx` arquivos no local de destino usando o Visual Studio ou seu editor de texto de escolha. Durante a pré-compilação para a implantação com uma interface de usuário atualizável, as páginas do ASP.NET no diretório do local de destino contêm a mesma marcação exatamente como os arquivos correspondentes no site.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Pré-compilação para implantação com uma Interface do usuário não atualizável

A ferramenta do compilador ASP.NET também pode ser usada para pré-compilar um site para implantação com uma interface do usuário não atualizável. Pré-compilar o site com uma interface do usuário não atualizável funciona bem como pré-compilar com uma interface do usuário atualizável, a principal diferença é que as páginas ASP.NET, controles de usuário e as páginas mestras no diretório de destino são removidas da sua marcação. Para pré-compilar um site para a implantação com uma interface do usuário não atualizável, escolha a opção de Publicar Site no menu Build, mas desmarque a opção "Permitir que este site pré-compilado seja atualizado" (consulte **Figura 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Figura 4**: desmarque a opção "Permitir que este site pré-compilado seja atualizado" opção para pré-compilar com um atualizável não interface do usuário  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-cs/_static/image12.png))

**Figura 5** mostra a pasta do local de destino depois de pré-compilar com uma interface do usuário não atualizável.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Figura 5**: A pasta do local de destino para a implantação com uma interface do usuário não atualizável  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-cs/_static/image15.png))

Comparar **Figura 3** à **Figura 5**. Embora as duas pastas podem parecer idênticas, observe que a pasta não atualizável de interface do usuário não tem a página mestra, `Site.master`. E enquanto **Figura 5** inclui várias páginas ASP.NET, se você exibir o conteúdo desses arquivos, você verá que eles já foram removidos da sua marcação declarativa e substituídos pelo texto do espaço reservado: "Este é um arquivo de marcador gerado pelo a ferramenta de pré-compilação e não deve ser excluído! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Figura 5**: A marcação declarativa foi removida das páginas do ASP.NET

O `Bin` pastas **figuras 3** e **5** diferem muito mais. Além dos assemblies, o `Bin` pasta no **Figura 5** inclui um `.compiled` arquivo para cada página ASP.NET, o controle de usuário e a página mestra.

Pré-compilar um site com uma interface do usuário não atualizável é útil em situações em que você não quer conteúdo as páginas do ASP.NET a ser modificado por pessoa ou empresa que instala ou que gerencia o site no ambiente de produção. Se você criar um aplicativo web ASP.NET que você vende para os clientes instalem em seus próprios servidores web, você talvez queira garantir que eles não modificam a aparência do seu site editando diretamente o `.aspx` páginas você enviá-las. Por meio da pré-compilação seu site com uma interface do usuário não atualizável, você enviar o espaço reservado `.aspx` páginas como parte da instalação, impedindo que os clientes de examinar ou modificar seu conteúdo.

### <a name="precompiling-from-the-command-line"></a>Pré-compilação da linha de comando

Nos bastidores, a caixa de diálogo Publish Web Site do Visual Studio chama a ferramenta de compilação do ASP.NET (`aspnet_compiler.exe`) para pré-compilar o site. Como alternativa, você pode invocar essa ferramenta da linha de comando. Na verdade, se você usar o Visual Web Developer, em seguida, você precisará executar a ferramenta de compilador de linha de comando, como o menu de compilação do Visual Web Developer não inclui a opção de Publicar Site.

Para usar a ferramenta do compilador da linha de comando, iniciar, descartando à linha de comando e navegando até o diretório da estrutura, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Em seguida, insira a instrução a seguir na linha de comando:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

O comando acima inicia a ferramenta do compilador ASP.NET (`aspnet_compiler.exe`) e, por meio de `-p` alternar, instrui a pré-compilar o site raiz na *físico\_caminho\_para\_aplicativo*; Esse valor será algo como `C:\MySites\BookReviews`e deve ser delimitado por aspas.

O `-v` switch Especifica o diretório virtual do site. Se seu site estiver registrado como o site padrão na metabase do IIS, você pode omitir o `-p` alternar e especificar apenas o diretório virtual do aplicativo. Se você usar o `-p` alternar, continuar o valor de `-v` switch indica a raiz do site e é usado para resolver referências da raiz do aplicativo. Por exemplo, se você especificar um valor de `-v /MySite` , em seguida, faz referência no aplicativo para `~/path/file` será resolvido como `~/MySite/path/file`. Porque o site de resenhas de livros está localizado no diretório raiz em minha empresa de hospedagem de web eu ter usado a opção `-v /`.

O `-f` , se presente, instrui a ferramenta de compilação para substituir o *alvo\_local\_pasta* diretório se ele já existe. Se você omitir o `-f` switch e a pasta do local de destino já existir, a ferramenta de compilação será encerrado com o erro: "Erro ASPRUNTIME: O diretório de destino não está vazio. Exclua-o manualmente ou escolha um destino diferente."

O `-u` switch, se presente, informa a ferramenta para criar uma interface de usuário atualizável. Omita esta opção para pré-compilar o site com uma interface do usuário não atualizável.

Por fim, o *destino\_local\_pasta* é o caminho físico para o diretório local de destino; esse valor será algo como `C:\MySites\Output\BookReviews`e deve ser delimitado por aspas.

## <a name="deploying-the-precompiled-website"></a>Implantando o site pré-compilado

Neste ponto, nós vimos como usar a ferramenta de compilação do ASP.NET para pré-compilar um site usando as duas as opções de interface do usuário atualizáveis e não atualizável. No entanto, nossos exemplos até agora tem pré-compilado o site para uma pasta local e não para o ambiente de produção. A boa notícia é que a implantação de site pré-compilado é muito simples e pode ser feito por meio do Visual Studio ou algum outro mecanismo de cópia de arquivo, como de um cliente FTP autônomo.

A caixa de diálogo Publish Web Site (mostrada pela primeira vez no **Figura 1**) tem uma opção de local de destino, que indica onde os arquivos de site pré-compilado são copiados. Esse local pode ser um servidor web remoto ou o servidor FTP. Inserir um servidor remoto nesta caixa de texto pré-compila e implanta o site para o servidor especificado em uma única etapa. Como alternativa, você pode pré-compilar o site em uma pasta local e, em seguida, copie manualmente o conteúdo dessa pasta para o ambiente de produção por meio de FTP ou algum outro processo.

Ter o site pré-compilado implantado automaticamente por meio da caixa de diálogo Publish Web Site do Visual Studio é útil para sites simples em que não há nenhuma diferença de configuração entre ambientes de desenvolvimento e produção. No entanto, como observado na [ *diferenças de configuração comuns entre desenvolvimento e produção* tutorial](common-configuration-differences-between-development-and-production-cs.md) não é incomum para essas diferenças de existir. Por exemplo, o aplicativo web de resenhas de livros usa um banco de dados diferente no ambiente de produção que no ambiente de desenvolvimento. Quando o Visual Studio publica o site em um servidor remoto cegamente copia as informações do arquivo de configuração no ambiente de desenvolvimento.

Para sites com diferenças de configuração entre ambientes de desenvolvimento e produção pode ser melhor pré-compilar o site para um diretório local, copie os arquivos de configuração específicas de produção e, em seguida, copie o conteúdo da saída pré-compilado como produção.

Para relembrar copiando arquivos do ambiente de desenvolvimento para o ambiente de produção, consulte a [ *Implantando seu site usando um cliente FTP* ](deploying-your-site-using-an-ftp-client-cs.md) e [  *Implantar seu site usando o Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) tutoriais.

## <a name="summary"></a>Resumo

ASP.NET oferece suporte a dois modos de compilação: automático e explícito. Conforme discutido nos tutoriais anteriores, o Web Application Projects (WAPs) usar compilação explícita enquanto o Web Site Projects (WSPs) usar a compilação automática, por padrão. No entanto, é possível compilar explicitamente um WSP antes da implantação usando a ferramenta de compilação do ASP.NET.

Este tutorial se concentra em pré-compilação da ferramenta de compilação para o suporte à implantação. Durante a pré-compilação para implantação, a ferramenta de compilação cria uma pasta de local de destino, compila o código-fonte do aplicativo web especificado e copia esses compilado assemblies e os arquivos de conteúdo para a pasta do local de destino. A ferramenta de compilação pode ser configurada para criar uma interface de usuário atualizável ou não atualizável. Quando pré-compilar com uma opção de interface do usuário não atualizável, a marcação declarativa nos arquivos de conteúdo é removida. Em resumo, pré-compilação permite que você implante seu aplicativo baseado no projeto de Site sem incluir quaisquer arquivos de código-fonte e com a marcação declarativa removida, se desejado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Pré-compilação de Site da Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Code-behind e a compilação do ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Pré-compilação no ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opções de Site pré-compilado no ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-elmah-cs.md)
> [Próximo](users-and-roles-on-the-production-website-cs.md)
