---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: "Pré-compilação seu site (VB) | Microsoft Docs"
author: rick-anderson
description: "O Visual Studio oferece aos desenvolvedores do ASP.NET dois tipos de projetos: projetos de aplicativo da Web (WAPs) e projetos de Site (WSPs). Um dos betwe principais diferenças..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: 7cc487aa5276c601fed632e82d7b6d32d1b53b58
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="precompiling-your-website-vb"></a>Pré-compilação seu site (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> O Visual Studio oferece aos desenvolvedores do ASP.NET dois tipos de projetos: projetos de aplicativo da Web (WAPs) e projetos de Site (WSPs). Uma das principais diferenças entre os dois tipos de projeto é que WAPs devem ter o código compilado explicitamente antes da implantação, enquanto o código em um WSP pode ser compilado automaticamente no servidor web. No entanto, é possível pré-compilar um WSP antes da implantação. Este tutorial explora os benefícios de pré-compilação e mostra como pré-compilar um site de dentro do Visual Studio e da linha de comando.


## <a name="introduction"></a>Introdução

O Visual Studio oferece aos desenvolvedores do ASP.NET dois diferentes tipos de projeto: projetos de aplicativo da Web (WAP) e projetos de Site da Web (WSP). Uma das principais diferenças entre esses tipos de projeto é que exigem WAPs *compilação explícita do* enquanto usam WSPs *compilação automática*, por padrão. Com WAPs, você deve compilar código do aplicativo da web em um único assembly, que é criado no site de `Bin` pasta. Implantação envolve copiar o conteúdo de marcação (a `.aspx.ascx`, e `.master` arquivos) no projeto, juntamente com o assembly no `Bin` pasta; o code-behind próprios arquivos de classe não precisam ser implantados. Por outro lado, você deve implantar WSPs copiando as páginas de marcação e suas classes de code-behind correspondente para o ambiente de produção. As classes de code-behind são compiladas sob demanda no servidor web.

> [!NOTE]
> Consulte a seção "Explícita compilação em comparação com compilação automática" o [ *determinar quais arquivos precisam ser implantados* tutorial](determining-what-files-need-to-be-deployed-vb.md) para obter mais informações sobre as diferenças entre o projeto modelos de compilação explícita e automática e como o modelo de compilação afeta a implantação.


A opção de compilação automática é simple de usar. Não há nenhuma etapa de compilação explícita e somente os arquivos que foram modificados necessidade para ser implantado, enquanto a compilação explícita exige Implantando as páginas alteradas marcação e o assembly compilado apenas. No entanto, a implantação automática tem duas possíveis desvantagens:

- Porque as páginas devem ser compiladas automaticamente quando elas são acessadas pela primeira vez, pode haver um atraso curto mas perceptível quando uma página ASP.NET é solicitada pela primeira vez após ser implantado.
- Compilação automática requer que ambos o declarativo marcação e código-fonte esteja presente no servidor web. Isso pode ser uma opção atraente se você planeja vender o aplicativo web aos clientes que ele será instalado em seus servidores web.

Se um dos dois acima limitações são separadores de negócio, você pode alterne para o modelo WAP ou *pré-compilar* WSP antes da implantação. Este tutorial examina as opções de pré-compilação mais adequadas para um site hospedado e orienta durante o processo de compilação e implantação de um site pré-compilado.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Uma visão geral de geração de código ASP.NET e compilação

Antes de examinarmos as opções de pré-compilação disponíveis, vamos primeiro falar sobre a geração de código e a compilação que ocorre quando uma página ASP.NET é solicitada pela primeira vez, desde que ele foi criado ou atualizado pela última vez. Como você sabe, páginas ASP.NET consistem em duas partes: marcação declarativa no `.aspx` arquivo; e uma parte do código fonte, normalmente em um arquivo de classe lógica separada (`.aspx.vb`). As etapas executadas pelo tempo de execução quando uma página ASP.NET é solicitada depende do modelo de compilação do aplicativo.

Com WAPs, o código-fonte das páginas deve ser compilado explicitamente em um único assembly antes de serem implantados. Durante a implantação, esse assembly e as várias páginas de marcação são copiadas para o ambiente de produção. Quando uma solicitação chega ao servidor da web para uma página ASP.NET, o tempo de execução cria uma instância da classe de code-behind da página e chama seu `ProcessRequest` método, que inicia o ciclo de vida da página e, em última análise, gera o conteúdo da página, que é retornado ao o solicitante. O tempo de execução pode trabalhar com a classe de code-behind da página ASP.NET porque a classe code-behind já foi compilada em um assembly antes da implantação.

Com WSPs e compilação automática, não há nenhuma etapa de compilação explícita antes da implantação. Em vez disso, a implantação envolve copiar declarativa e o conteúdo do código de origem para o ambiente de produção. Quando uma solicitação chega ao servidor da web para uma página ASP.NET pela primeira vez como a página foi criada ou atualizada pela última vez, o tempo de execução deve primeiro compilar a classe code-behind em um assembly. Este assembly compilado é salvo na pasta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, embora o local dessa pasta pode ser personalizado por meio de `<pages>` elemento `Web.config`. Porque o assembly é salvo em disco, ele não precisa ser recompilado em solicitações subsequentes para a mesma página.

> [!NOTE]
> Como se esperaria, há um ligeiro atraso ao solicitar uma página pela primeira vez (ou pela primeira vez, já que ela tiver sido alterada) em um site que usa compilação automática que usa um momento para compilar o código da página e salve o assembly resultante para o servidor disco.


Em resumo, com a compilação explícita é necessário para compilar o código-fonte do site antes da implantação, salvando o tempo de execução de ter que executar essa etapa. Com a compilação automática o tempo de execução lida com a compilação do código-fonte das páginas, mas com um custo de inicialização de pequena para a primeira visita a página desde que ele foi criado ou atualizado pela última vez.

Mas e quanto a parte declarativa de páginas ASP.NET (o `.aspx` arquivo)? É claro que há uma relação entre o `.aspx` arquivos e o código em suas classes de code-behind, como os controles da Web definidos na marcação declarativa são acessíveis no código. Também é óbvio que o conteúdo de `.aspx` arquivos influencia bastante marcação renderizada gerada pela página. Como funciona o tempo de execução com o texto de HTML, e a sintaxe de controle da Web definido no `.aspx` arquivo para gerar a página solicitada do conteúdo renderizado?

Não quero muito seja desviado do assunto em detalhes de implementação de baixo nível, que variam entre WAPs e WSPs, mas resumindo o tempo de execução gera automaticamente um arquivo de classe que contém vários controles da Web como métodos e membros protegidos. Esse arquivo gerado é implementado como um *classe parcial* para a classe code-behind correspondente. ([Classes parciais](http://www.dotnet-guide.com/partialclasses.html) permitem que o conteúdo de uma única classe ser distribuídos em vários arquivos.) Portanto, a classe code-behind é definida em dois locais: no `.aspx.vb` arquivo que você cria e nessa classe gerada automaticamente criada pelo tempo de execução. Essa classe gerada automaticamente é armazenado no `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` pasta.

O importante tirar aqui é que, para uma página ASP.NET ser processado pelo tempo de execução ambos os seus declarativa e partes do código de origem devem ser compiladas em um assembly. Com WAPs, explicitamente, o código-fonte é compilado em um assembly antes da implantação, mas a marcação declarativa ainda deve ser convertida em código e compilada pelo tempo de execução no servidor web. Com WSPs usando compilação automática, o código-fonte e a marcação declarativa precisam ser compilado pelo servidor web.

É possível usar a compilação explícita com o modelo WSP. Você pode compilar explicitamente a parte do código fonte, como com o modelo WAP. Além disso, você também pode compilar a marcação declarativa.

## <a name="precompilation-options"></a>Opções de pré-compilação

O .NET Framework vem com um [ferramenta de compilação do ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) que permite que você compilar o código-fonte (e até mesmo o conteúdo) de um aplicativo ASP.NET criado usando o modelo WSP. Essa ferramenta foi lançada com o .NET Framework versão 2.0 e está localizada no `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` pasta; pode ser usado na linha de comando ou iniciado a partir do Visual Studio por meio da opção de publicar Web Site do menu Build.

A ferramenta de compilação oferece dois formulários gerais da compilação: in-loco pré-compilação pré-compilação para implantação. Com pré-compilação no local em que você executar o `aspnet_compiler.exe` ferramenta de linha de comando e especifique o caminho para o diretório virtual ou o caminho físico de um site que reside em seu computador. A ferramenta de compilação, em seguida, compila cada página ASP.NET no projeto, armazenar a versão compilada no `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` pasta como se as páginas de cada um foi visitadas pela primeira vez em um navegador. Pré-compilação in-loco pode acelerar a primeira solicitação feita ao recém-implantados páginas ASP.NET em seu site porque minimiza o tempo de execução da necessidade de executar essa etapa. No entanto, pré-compilação no local não é útil para a maioria dos sites hospedados porque ele requer que você seja capazes de executar programas a partir do servidor web de linha de comando. Esse nível de acesso não é permitida em ambientes de hospedagem compartilhados.

> [!NOTE]
> Para obter mais informações sobre pré-compilação in-loco, check-out [How To: Precompile ASP.NET Web Sites](https://msdn.microsoft.com/library/ms227972.aspx) e [pré-compilação do ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Em vez de compilar as páginas no site da Web para o `Temporary ASP.NET Files` pasta pré-compilação para implantação compila as páginas em um diretório de sua escolha e em um formato que pode ser implantado no ambiente de produção.

Há dois tipos de pré-compilação para implantação exploraremos neste tutorial: pré-compilação com uma interface de usuário atualizável e pré-compilação com uma interface de usuário não atualizável. Pré-compilação com uma interface de usuário atualizável deixa a marcação declarativa no `.aspx`, `.ascx`, e `.master` arquivos, permitindo assim que um desenvolvedor exibir e, se desejar, modifique a marcação declarativa no servidor de produção. Pré-compilação com uma interface de usuário não atualizável gera `.aspx` páginas que são void de qualquer conteúdo e remove `.ascx` e `.master` arquivos, assim, ocultar a marcação declarativa e proibindo um desenvolvedor de alterá-la da ambiente de produção.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Pré-compilação para implantação com uma Interface de usuário atualizável

É a melhor maneira de entender pré-compilação para implantação ver um exemplo em ação. Vamos pré-compilar o WSP revisões de catálogo para implantação usando uma interface de usuário atualizável. A ferramenta de compilação do ASP.NET pode ser invocada no menu de compilação do Visual Studio ou na linha de comando. Esta seção examina usando a ferramenta de dentro do Visual Studio; a seção "pré-compilação da linha de comando" examina a execução da ferramenta do compilador de linha de comando.

Abrir o catálogo de revisão WSP no Visual Studio, vá para o menu de compilação e selecione a opção de menu Publicar Site. Isso inicia a caixa de diálogo Publicar Web Site (consulte **Figura 1**), onde você pode especificar o local de destino, ou não a interface do usuário do site pré-compilado é atualizável e outras opções de ferramenta do compilador. O local de destino pode ser um servidor web remoto ou servidor FTP, mas por enquanto, escolha uma pasta no disco rígido do computador. Como queremos pré-compilar o site com uma interface de usuário atualizável, deixe a caixa de seleção "Permitir que este site pré-compilado seja atualizado" marcada e clique em Okey.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Figura 1**: A ferramenta de compilação do ASP.NET será pré-compilar o site para o local de destino especificado  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> A opção de publicar Web Site no menu de compilação não está disponível no Visual Web Developer. Se você estiver usando o Visual Web Developer, será necessário usar a versão de linha de comando da ferramenta de compilação do ASP.NET, que é abordada na seção "pré-compilação da linha de comando".


Após o site de pré-compilação, navegue até o local de destino que você inseriu na caixa de diálogo Publicar Site. Dedique alguns momentos para comparar o conteúdo desta pasta com o conteúdo do seu site. **Figura 2** mostra a pasta do site revisões de livros. Observe que ele contém ambos `.aspx` e `.aspx.cs` arquivos. Além disso, observe que o `Bin` diretório inclui apenas um arquivo, `Elmah.dll`, que adicionamos a [tutorial anterior](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Figura 2**: O diretório do projeto contém `.aspx` e `.aspx.cs` arquivos; o `Bin` pasta inclui apenas`Elmah.dll`  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image6.png))

**Figura 3** mostra a pasta do local de destino cujo conteúdo foram criado pela ferramenta de compilação do ASP.NET. Esta pasta não contém quaisquer arquivos code-behind. Além disso, essa pasta `Bin` diretório inclui vários assemblies e dois `.compiled` arquivos além do `Elmah.dll` assembly.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Figura 3**: A pasta do local de destino inclui os arquivos de implantação  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image9.png))

Ao contrário de compilação explícita em WAPs, a pré-compilação para o processo de implantação não cria um assembly para o site inteiro. Em vez disso, ele processa em lotes juntos várias páginas em cada assembly. Ele também compila o `Global.asax` o arquivo (se houver) em seu próprio assembly, bem como todas as classes no `App_Code` pasta. Os arquivos que contêm a marcação declarativa para ASP.NET web páginas mestras, páginas e controles de usuário (`.aspx`, `.ascx`, e `.master` arquivos, respectivamente) são copiados como-é o diretório local de destino. Da mesma forma, o `Web.config` arquivo é copiado diretamente, juntamente com quaisquer arquivos estáticos, como arquivos PDF, imagens e classes CSS. Para obter uma descrição mais formal de como a ferramenta de compilação lida com vários tipos de arquivo, consulte [tratamento de arquivo durante pré-compilação ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Você pode instruir a ferramenta de compilação para criar um assembly por página ASP.NET, controle de usuário ou página mestra, marcando a caixa de seleção "Usado fixo de nomenclatura e assemblies de única página" na caixa de diálogo Publicar Site. Com cada página do ASP.NET compilada em seu próprio assembly permite um controle mais refinado sobre implantação. Por exemplo, se você atualizou uma única página web ASP.NET e necessárias para implantar essa alteração, você só precisa implantar essa página `.aspx` de arquivo e o assembly associado ao ambiente de produção. Consulte [como: gerar nomes fixos com a ferramenta de compilação do ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) para obter mais informações.


O diretório de local de destino também contém um arquivo que não fazia parte do projeto da web pré-compilado, ou seja, `PrecompiledApp.config`. Este arquivo informa o tempo de execução do ASP.NET que o aplicativo foi pré-compilado e se ele foi pré-compilado com uma interface do usuário atualizável ou meio-dia atualizável.

Por fim, reserve um tempo para abrir um o `.aspx` arquivos no local de destino usando o Visual Studio ou seu editor de texto de escolha. Quando pré-compilar para implantação com uma interface de usuário atualizável, as páginas do ASP.NET no diretório local de destino contêm a mesma marcação exatamente como os arquivos correspondentes no site.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Pré-compilação para implantação com uma Interface do usuário não atualizável

A ferramenta do compilador ASP.NET também pode ser usada para pré-compilar um site para implantação com uma interface do usuário não atualizável. Pré-compilar o site com uma interface do usuário não atualizável funciona praticamente como pré-compilar com uma interface do usuário atualizável, a principal diferença é que as páginas ASP.NET, controles de usuário e páginas mestras no diretório de destino são removidas de suas marcações. Para pré-compilar um site para implantação com uma interface do usuário não atualizável, escolha a opção de Publicar Site no menu compilar, mas desmarque a opção "Permitir que este site pré-compilado seja atualizado" (consulte **Figura 4**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Figura 4**: desmarque "Permitir que este site pré-compilado seja atualizado" opção para pré-compilar com um não atualizável interface do usuário  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image12.png))

**Figura 5** mostra a pasta do local de destino após a pré-compilação com uma interface de usuário não atualizável.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Figura 5**: A pasta do local de destino para a implantação com uma interface do usuário não atualizável  
 ([Clique para exibir a imagem em tamanho normal](precompiling-your-website-vb/_static/image15.png))

Comparar **Figura 3** para **Figura 5**. Embora as duas pastas podem parecer idênticas, observe que a pasta não atualizável de interface do usuário não tem a página mestra, `Site.master`. E enquanto **Figura 5** inclui várias páginas do ASP.NET, se você exibir o conteúdo desses arquivos, você verá que já foi removidos de sua marcação declarativa e substituídas o texto do espaço reservado: "Este é um arquivo marcador gerado pelo a ferramenta de pré-compilação e não deve ser excluído! "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Figura 5**: A marcação declarativa foi removida das páginas ASP.NET

O `Bin` pastas **figuras 3** e **5** diferem muito mais. Além dos assemblies, o `Bin` pasta **Figura 5** inclui um `.compiled` arquivo para cada página ASP.NET, o controle de usuário e a página mestra.

Pré-compilação de um site com uma interface do usuário não atualizável é útil em situações onde você não deseja que o conteúdo das páginas do ASP.NET para ser modificado por pessoa ou empresa que instala ou gerencia o site no ambiente de produção. Se você criar um aplicativo web ASP.NET que vendem para os clientes instalem em seus próprios servidores web, convém garantir que eles não modifique a aparência do seu site editando diretamente o `.aspx` páginas você enviá-las. Pré-compilando seu site com uma interface do usuário não atualizável, envie o espaço reservado `.aspx` páginas como parte da instalação, impedindo que os clientes de examinar ou modificar seu conteúdo.

### <a name="precompiling-from-the-command-line"></a>Pré-compilação na linha de comando

Nos bastidores, a caixa de diálogo Publicar Web Site do Visual Studio chama a ferramenta de compilação do ASP.NET (`aspnet_compiler.exe`) para pré-compilar o site. Como alternativa, você pode chamar essa ferramenta da linha de comando. Na verdade, se você usar o Visual Web Developer, em seguida, você precisará executar a ferramenta de compilador de linha de comando, como o menu de compilação do Visual Web Developer não inclui a opção de Publicar Site.

Para usar a ferramenta do compilador de linha de comando, inicie descartando à linha de comando e navegando até o diretório do framework, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Em seguida, digite a seguinte instrução na linha de comando:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

O comando acima inicia a ferramenta de compilador do ASP.NET (`aspnet_compiler.exe`) e, por meio de `-p` alternar, instrui-lo para pré-compilar o site com raiz em *físico\_caminho\_para\_aplicativo*; Esse valor será algo como `C:\MySites\BookReviews`e devem ser delimitados por aspas.

O `-v` opção especifica o diretório virtual do site. Se seu site estiver registrado como o site padrão na metabase do IIS, você pode omitir o `-p` alternar e especificar apenas o diretório virtual do aplicativo. Se você usar o `-p` alternar, continuar o valor de `-v` switch indica a raiz do site e é usado para resolver referências da raiz do aplicativo. Por exemplo, se você especificar um valor de `-v /MySite` , em seguida, faz referência no aplicativo para `~/path/file` será resolvido como `~/MySite/path/file`. Porque o site de análises de catálogo está localizado no diretório raiz em minha empresa de hospedagem de web usei a opção `-v /`.

O `-f` , se presente, instrui a ferramenta de compilação para substituir o *destino\_local\_pasta* diretório se ele já existe. Se você omitir o `-f` switch e a pasta do local de destino já existir, a ferramenta de compilação será encerrado com o erro: "Erro ASPRUNTIME: O diretório de destino não está vazio. Exclua-o manualmente ou escolha um destino diferente."

O `-u` comutador, se presente, informa a ferramenta para criar uma interface de usuário atualizável. Omita essa opção para pré-compilar o site com uma interface de usuário não atualizável.

Por fim, o *destino\_local\_pasta* é o caminho físico para o diretório local de destino, esse valor será algo como `C:\MySites\Output\BookReviews`e devem ser delimitados por aspas.

## <a name="deploying-the-precompiled-website"></a>Implantando o site pré-compilado

Neste ponto, vimos como usar a ferramenta de compilação do ASP.NET para pré-compilar um site usando as opções de interface de usuário atualizáveis e não atualizável. No entanto, nossos exemplos até agora tem pré-compilado o site para uma pasta local e não para o ambiente de produção. A boa notícia é que implantar o site pré-compilado é muito simples e pode ser feito por meio do Visual Studio ou algum outro mecanismo de cópia de arquivo, como de um cliente FTP autônomo.

A caixa de diálogo Publicar Web Site (mostrado pela primeira vez no **Figura 1**) tem uma opção de local de destino, que indica onde os arquivos do site pré-compilado são copiados. Esse local pode ser um servidor web remoto ou servidor FTP. Inserir um servidor remoto nesta caixa de texto pré-compila e implanta o site para o servidor especificado em uma única etapa. Como alternativa, você pode pré-compilar o site para uma pasta local e, em seguida, copiar o conteúdo da pasta para o ambiente de produção por meio de FTP ou algum outro processo.

Com o site pré-compilado automaticamente implantado por meio da caixa de diálogo Publicar Web Site do Visual Studio é útil para sites simples em que não há nenhuma diferença de configuração entre os ambientes de desenvolvimento e produção. No entanto, conforme indicado no [ *diferenças de configuração comuns entre o desenvolvimento e produção* tutorial](common-configuration-differences-between-development-and-production-vb.md) não é incomum para essas diferenças de existir. Por exemplo, o aplicativo web de catálogo revisões usa um banco de dados diferente no ambiente de produção que no ambiente de desenvolvimento. Quando o Visual Studio publica o site em um servidor remoto cegamente copia as informações do arquivo de configuração no ambiente de desenvolvimento.

Para sites com diferenças de configuração entre os ambientes de desenvolvimento e produção pode ser melhor pré-compilar o site para um diretório local, copie os arquivos de configuração específicas de produção e, em seguida, copie o conteúdo da saída pré-compilado para produção.

Para uma atualização sobre a cópia de arquivos do ambiente de desenvolvimento para o ambiente de produção, consulte o [ *Implantando seu site usando um cliente FTP* ](deploying-your-site-using-an-ftp-client-vb.md) e [  *Implantação de seu site usando o Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) tutoriais.

## <a name="summary"></a>Resumo

ASP.NET oferece suporte a dois modos de compilação: automática e explícitas. Como discutido nos tutoriais anteriores, projetos de aplicativo da Web (WAPs) usar compilação explícita enquanto projetos de Site (WSPs) usar compilação automática, por padrão. No entanto, é possível compilar explicitamente um WSP antes da implantação usando a ferramenta de compilação do ASP.NET.

Este tutorial é focado em pré-compilação da ferramenta de compilação para suporte de implantação. Quando pré-compilar para implantação, a ferramenta de compilação cria uma pasta de local de destino, compila o código-fonte do aplicativo da web especificado e copia esses compilado assemblies e os arquivos de conteúdo para a pasta do local de destino. A ferramenta de compilação pode ser configurada para criar uma interface de usuário atualizável ou não atualizável. Quando pré-compilar com uma opção de interface do usuário não atualizável, a marcação declarativa nos arquivos de conteúdo é removida. Em resumo, pré-compilação permite implantar seu aplicativo com base em projeto de Site sem incluir os arquivos de código fonte e com a marcação declarativa removido, se desejado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Pré-compilação de Site da Web do ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Code-behind e compilação do ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Pré-compilação do ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opções de Site de pré-compilação do ASP.NET](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[Anterior](logging-error-details-with-elmah-vb.md)
[Próximo](users-and-roles-on-the-production-website-vb.md)
