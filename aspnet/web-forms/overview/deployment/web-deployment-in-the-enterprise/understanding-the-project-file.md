---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Noções básicas sobre o arquivo de projeto | Microsoft Docs
author: jrjlee
description: Arquivos de projeto do Microsoft Build Engine (MSBuild) são a base do processo de compilação e implantação. Este tópico começa com uma visão geral conceitual do MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 89c5c7906ccfc453195b788cbc6393dc74cda1fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377198"
---
<a name="understanding-the-project-file"></a>Noções básicas sobre o arquivo de projeto
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Arquivos de projeto do Microsoft Build Engine (MSBuild) são a base do processo de compilação e implantação. Este tópico começa com uma visão geral conceitual do MSBuild e o arquivo de projeto. Ele descreve os principais componentes que você vai se deparar com quando você trabalha com arquivos de projeto, e ele funciona por meio de um exemplo de como você pode usar arquivos de projeto para implantar aplicativos do mundo real.
> 
> O que você aprenderá:
> 
> - Como o MSBuild usa arquivos de projeto do MSBuild para compilar projetos.
> - Como o MSBuild se integra com tecnologias de implantação, como a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web).
> - Como compreender os componentes principais de um arquivo de projeto.
> - Como você pode usar arquivos de projeto para criar e implantar aplicativos complexos.


## <a name="msbuild-and-the-project-file"></a>O MSBuild e o arquivo de projeto

Quando você cria e compilar soluções no Visual Studio, o Visual Studio usa o MSBuild para compilar cada projeto na solução. Cada projeto do Visual Studio inclui um arquivo de projeto do MSBuild, com uma extensão de arquivo que reflete o tipo de projeto&#x2014;por exemplo, um projeto c# (. csproj), um projeto do Visual Basic.NET (. vbproj) ou um projeto de banco de dados (. dbproj). Para criar um projeto, o MSBuild deve processar o arquivo de projeto associado ao projeto. O arquivo de projeto é um documento XML que contém todas as informações e instruções que precisa de MSBuild para compilar seu projeto, como o conteúdo a ser incluído, requisitos de plataforma, informações de controle de versão, servidor web ou configurações do servidor de banco de dados e o tarefas que devem ser executadas.

Arquivos de projeto MSBuild se baseiam os [esquema XML do MSBuild](https://msdn.microsoft.com/library/5dy88c2e.aspx), e assim o processo de compilação é totalmente aberto e transparente. Além disso, você não precisa instalar o Visual Studio para usar o mecanismo do MSBuild&#x2014;executável MSBuild.exe faz parte do .NET Framework, e você pode executá-lo em um prompt de comando. Como desenvolvedor, você pode criar seus próprios arquivos de projeto do MSBuild, usando o esquema XML do MSBuild, para impor o controle refinado e sofisticado sobre como seus projetos são criados e implantados. Esses arquivos de projeto personalizados funcionam exatamente da mesma forma que os arquivos de projeto que o Visual Studio gera automaticamente.

> [!NOTE]
> Você também pode usar arquivos de projeto MSBuild com o serviço Team Build no Team Foundation Server (TFS). Por exemplo, você pode usar arquivos de projeto em cenários de integração contínua (CI) para automatizar a implantação em um ambiente de teste quando o novo código é verificado. Para obter mais informações, consulte [Configurando o Team Foundation Server para a implantação automatizada de Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Convenções de nomenclatura de arquivo de projeto

Quando você cria seus próprios arquivos de projeto, você pode usar qualquer extensão de arquivo que você deseja. No entanto, para facilitar para outras pessoas entenderem suas soluções, você deve usar essas convenções comuns:

- Quando você cria um arquivo de projeto que compila projetos, use a extensão de. proj.
- Quando você cria um arquivo de projeto reutilizáveis para importar para outros arquivos de projeto, use a extensão. targets. Arquivos com uma extensão. targets normalmente não criam qualquer coisa em si, eles simplesmente contenham instruções que você pode importar para seus arquivos. proj.

### <a name="integration-with-deployment-technologies"></a>Integração com tecnologias de implantação

Se você já trabalhou com projetos de aplicativos web no Visual Studio 2010, como aplicativos web ASP.NET e aplicativos web do ASP.NET MVC, você saberá que esses projetos incluem suporte interno para empacotar e implantar o aplicativo web em um ambiente de destino. O **propriedades** páginas para esses projetos incluem **pacote/Publicar Web** e **empacotar/publicar SQL** guias que você pode usar para configurar como os componentes do seu aplicativos são empacotados e implantados. Isso mostra a **empacotar/Publicar Web** guia:

![](understanding-the-project-file/_static/image1.png)

A tecnologia subjacente através desses recursos é conhecida como Web Publishing Pipeline (WPP). O WPP essencialmente traz MSBuild e [implantação da Web](https://go.microsoft.com/?linkid=9805122) para fornecer um processo de compilação, pacote e implantação completo de seus aplicativos web.

A boa notícia é que você pode tirar proveito dos pontos de integração que o WPP fornece quando você cria arquivos de projeto personalizados para projetos web. Você pode incluir instruções de implantação no arquivo de projeto, que permite que você compile seus projetos, criar pacotes de implantação da web e instalar esses pacotes em servidores remotos por meio de um único arquivo de projeto e uma única chamada para o MSBuild. Você também pode chamar todos os outros executáveis como parte do processo de compilação. Por exemplo, você pode executar a ferramenta de linha de comando VSDBCMD.exe para implantar um banco de dados de um arquivo de esquema. Ao longo deste tópico, você verá como você pode tirar proveito desses recursos para atender aos requisitos de seus cenários de implantação do enterprise.

> [!NOTE]
> Para obter mais informações sobre como funciona o processo de implantação do aplicativo web, consulte [visão geral de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>A anatomia de um arquivo de projeto

Antes de olhar mais detalhadamente o processo de compilação, vale a pena levando alguns minutos para se familiarizar com a estrutura básica de um arquivo de projeto MSBuild. Esta seção fornece uma visão geral dos elementos mais comuns que você encontrará ao revisar, editar ou criar um arquivo de projeto. Em particular, você aprenderá:

- Como usar *propriedades* para gerenciar variáveis para o processo de compilação.
- Como usar *itens* para identificar as entradas para o processo de compilação, como arquivos de código.
- Como usar *destinos* e *tarefas* para fornecer instruções de execução para o MSBuild, usando *propriedades* e *itens* definido em outro lugar o arquivo de projeto.

Isso mostra a relação entre os principais elementos em um arquivo de projeto do MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>O elemento de projeto

O [projeto](https://msdn.microsoft.com/library/bcxfsh87.aspx) é o elemento raiz de cada arquivo de projeto. Além de identificar o esquema XML para o arquivo de projeto, o **projeto** elemento pode incluir atributos para especificar os pontos de entrada para o processo de compilação. Por exemplo, nos [solução de exemplo do Gerenciador de contatos](the-contact-manager-solution.md), o *Publish.proj* arquivo Especifica que a compilação deve começar chamando o destino chamado **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Propriedades e condições

Um arquivo de projeto geralmente precisa fornecer muitas partes diferentes de informações para compilar e implantar seus projetos com êxito. Essas duas informações pode incluir nomes de servidor, cadeias de caracteres de conexão, credenciais, configurações de build, os caminhos de arquivo de origem e de destino e qualquer outra informação que você deseja incluir para dar suporte à personalização. Em um arquivo de projeto, as propriedades devem ser definidas dentro de um [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) elemento. Propriedades do MSBuild consistem em pares chave-valor. Dentro de **PropertyGroup** elemento, o nome do elemento define a chave de propriedade e o conteúdo do elemento define o valor da propriedade. Por exemplo, você pode definir as propriedades nomeadas **nome_do_servidor** e **ConnectionString** para armazenar uma cadeia de caracteres de nome e a conexão de servidor estático.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Para recuperar um valor de propriedade, você pode usar o formato <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Por exemplo, para recuperar o valor da <strong>ServerName</strong> propriedade, você digitaria:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Você verá exemplos de como e quando usar os valores de propriedade neste tópico.


Incorporando informações como propriedades estáticas em um arquivo de projeto nem sempre é a abordagem ideal para gerenciar o processo de compilação. Em muitos cenários, você desejará obter as informações de outras fontes ou maior autonomia ao usuário para fornecer as informações do prompt de comando. MSBuild permite que você especifique qualquer valor de propriedade como um parâmetro de linha de comando. Por exemplo, o usuário pode fornecer um valor para **ServerName** quando ele ou ela é executada MSBuild.exe da linha de comando.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Para obter mais informações sobre os argumentos e opções podem ser usadas com MSBuild.exe, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


Você pode usar a mesma sintaxe de propriedade para obter os valores das variáveis de ambiente e propriedades de projeto internos. Muitas propriedades mais usadas são definidas por você, e você pode usá-los em seus arquivos de projeto, incluindo o nome do parâmetro relevante. Por exemplo, para recuperar a atual plataforma de projeto&#x2014;por exemplo, **x86** ou **AnyCpu**&#x2014;você pode incluir o **$ (plataforma)** referência de propriedade no o arquivo de projeto. Para obter mais informações, consulte [Macros para compilar comandos e propriedades](https://msdn.microsoft.com/library/c02as0cs.aspx), [propriedades de projeto comuns do MSBuild](https://msdn.microsoft.com/library/bb629394.aspx), e [propriedades reservadas](https://msdn.microsoft.com/library/ms164309.aspx).

Propriedades são geralmente usadas em conjunto com *condições*. A maioria dos elementos do MSBuild dão suporte a **condição** atributo, que permite que você especifique os critérios dos quais o MSBuild deve avaliar o elemento. Por exemplo, considere esta definição de propriedade:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Quando o MSBuild processa essa definição de propriedade, ele primeiro verifica para ver se um **$(OutputRoot)** valor da propriedade está disponível. Se o valor da propriedade estiver em branco&#x2014;em outras palavras, o usuário ainda não forneceu um valor para essa propriedade&#x2014;a condição for avaliada como **verdadeira** e o valor da propriedade é definido como **... \Publish\Out**. Se o usuário forneceu um valor para essa propriedade, a condição for avaliada como **falsos** e o valor da propriedade estática não é usado.

Para obter mais informações sobre as diferentes maneiras em que você pode especificar condições, consulte [condições do MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Itens e grupos de itens

Uma das funções importantes do arquivo de projeto é definir as entradas para o processo de compilação. Normalmente, essas entradas são arquivos&#x2014;codificar arquivos, arquivos de configuração, arquivos de comando e outros arquivos que você precisa processar ou copiar como parte do processo de compilação. No esquema de projeto MSBuild, essas entradas são representadas por [Item](https://msdn.microsoft.com/library/ms164283.aspx) elementos. Em um arquivo de projeto, itens devem ser definidas dentro de um [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) elemento. Assim como **propriedade** elementos, você pode nomear um **Item** elemento você quiser. No entanto, você deve especificar um **Include** atributo para identificar o arquivo ou curinga que representa o item.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Especificando vários **Item** elementos com o mesmo nome, você está criando efetivamente uma lista nomeada de recursos. É uma boa maneira de ver isso em ação dar uma olhada dentro de um dos arquivos de projeto que o Visual Studio cria. Por exemplo, o *ContactManager.Mvc.csproj* arquivo na solução de exemplo inclui muitos dos grupos de itens, cada um com vários nomes idênticos **Item** elementos.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Dessa forma, o arquivo de projeto é instruir o MSBuild para construir as listas de arquivos que precisam ser processados da mesma forma&#x2014;o **referência** lista inclui assemblies que devem estar em vigor para uma compilação bem-sucedida, o  **Compile** lista inclui arquivos de código que devem ser compilados, e o **conteúdo** lista inclui recursos que devem ser copiados inalterados. Vamos examinar como o processo de compilação faz referência e usa esses itens posteriormente neste tópico.

Elementos de item também podem incluir [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) elementos filho. Estes são pares de chave-valor definidos pelo usuário e essencialmente representam as propriedades que são específicas para aquele item. Por exemplo, muitos a **Compile** elementos de item no arquivo de projeto incluem **DependentUpon** elementos filho.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Além dos metadados de item criado pelo usuário, todos os itens são atribuídos a vários metadados comuns na criação. Para obter mais informações, consulte [Metadados de itens conhecidos](https://msdn.microsoft.com/library/ms164313.aspx).


Você pode criar **ItemGroup** elementos dentro do nível raiz **Project** elemento ou dentro de específica **destino** elementos. **ItemGroup** elementos também suportam **condição** atributos, que permite que você personalize as entradas para o processo de compilação acordo com as condições, como a configuração do projeto ou plataforma.

### <a name="targets-and-tasks"></a>Destinos e tarefas

No esquema de MSBuild, uma [tarefa](https://msdn.microsoft.com/library/77f2hx1s.aspx) elemento representa uma instrução de compilação individual (ou tarefas). MSBuild inclui uma variedade de tarefas predefinidas. Por exemplo:

- O **cópia** tarefa copia arquivos para um novo local.
- O **Csc** tarefa chama o compilador do Visual c#.
- O **Vbc** tarefa chama o compilador do Visual Basic.
- O **Exec** tarefa executa um programa especificado.
- O **mensagem** tarefa grava uma mensagem para um agente.

> [!NOTE]
> Para obter detalhes completos das tarefas que estão disponíveis fora da caixa, consulte [referência de tarefa do MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Para obter mais informações sobre tarefas, incluindo como criar suas próprias tarefas personalizadas, consulte [tarefas do MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).


Tarefas sempre devem estar contidas dentro [destino](https://msdn.microsoft.com/library/t50z2hka.aspx) elementos. Um **destino** elemento é um conjunto de uma ou mais tarefas que são executadas sequencialmente, e um arquivo de projeto pode conter vários destinos. Quando você deseja executar uma tarefa ou um conjunto de tarefas, você chama o destino em que os contém. Por exemplo, suponha que você tenha um arquivo de projeto simples que registra uma mensagem.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Você pode invocar o destino da linha de comando, usando o **/t** para especificar o destino.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Como alternativa, você pode adicionar um **DefaultTargets** atributo para o **projeto** elemento para especificar os destinos que você deseja invocar.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


Nesse caso, você não precisa especificar o destino da linha de comando. Você pode simplesmente especificar o arquivo de projeto e invocará o MSBuild a **FullPublish** destino para você.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Destinos e tarefas podem incluir **condição** atributos. Como tal, você pode optar por omitir destinos inteiros ou tarefas individuais se determinadas condições forem atendidas.

Em geral, quando você cria tarefas úteis e destinos, você precisará consultar as propriedades e os itens que você definiu em outro lugar no arquivo de projeto:

- Para usar um valor de propriedade, digite <strong>$(</strong><em>PropertyName</em><strong>)</strong>, onde <em>PropertyName</em> é o nome da <strong>propriedade</strong> elemento ou o nome do parâmetro.
- Para usar um item, digite <strong>@(</strong><em>ItemName</em><strong>)</strong>, onde <em>ItemName</em> é o nome da <strong>Item</strong> elemento.

> [!NOTE]
> Lembre-se de que, se você criar vários itens com o mesmo nome, você está criando uma lista. Por outro lado, se você criar várias propriedades com o mesmo nome, o último valor de propriedade que você fornecer substituirá quaisquer propriedades anteriores com o mesmo nome&#x2014;uma propriedade pode conter apenas um único valor.


Por exemplo, nos *Publish.proj* de arquivos na solução de exemplo, dê uma olhada a **BuildProjects** destino.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


Neste exemplo, você pode observar esses pontos-chave:

- Se o **BuildingInTeamBuild** parâmetro for especificado e tem um valor de **verdadeiro**, nenhuma das tarefas desse destino será executada.
- O destino contém uma única instância de [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) tarefa. Essa tarefa permite criar outros projetos do MSBuild.
- O **ProjectsToBuild** item é passado para a tarefa. Este item pode representar uma lista de arquivos de projeto ou solução, tudo definidos pela **ProjectsToBuild** elementos dentro de um grupo de itens de item. Nesse caso, o **ProjectsToBuild** item refere-se a um arquivo de solução única.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Os valores de propriedade passados para o **MSBuild** tarefa incluir parâmetros nomeados **OutputRoot** e **configuração**. Se não estiverem, elas são definidas para os valores de parâmetro se eles forem fornecidos, ou valores de propriedade estática.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Você também pode ver que o **MSBuild** tarefa invoca um destino chamado **Build**. Esse é um dos vários destinos internos que são amplamente usados em arquivos de projeto do Visual Studio e estão disponíveis para você em seus arquivos de projeto personalizado, como **construir**, **Clean**, **recompilar**, e **publicar**. Você aprenderá mais sobre o uso de destinos e tarefas para controlar o processo de compilação e sobre o **MSBuild** de tarefa em particular, neste tópico.

> [!NOTE]
> Para obter mais informações sobre destinos, consulte [destinos do MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Divisão de arquivos de projeto para dar suporte a vários ambientes

Suponha que você deseja ser capaz de implantar uma solução em vários ambientes, como servidores de teste, preparação de plataformas e ambientes de produção. A configuração pode variar significativamente entre esses ambientes&#x2014;não apenas em termos de nomes de servidor, cadeias de caracteres de conexão e assim por diante, mas também potencialmente em termos de credenciais, as configurações de segurança e muitos outros fatores. Se você precisar fazer isso regularmente, não é realmente vantajoso editar várias propriedades em seu arquivo de projeto sempre que você alternar o ambiente de destino. Nem é uma solução ideal para exigir uma lista interminável de valores de propriedade a ser fornecida para o processo de compilação.

Felizmente, há uma alternativa. MSBuild permite dividir sua configuração de compilação em vários arquivos de projeto. Para ver como isso funciona, na solução de exemplo, observe que há dois arquivos de projeto personalizado:

- *Publish.proj*, que contém propriedades, itens, e os destinos que são comuns a todos os ambientes.
- *Env Dev.proj*, que contém as propriedades que são específicas para um ambiente de desenvolvedor.

Agora observe que o *Publish.proj* inclui um [importação](https://msdn.microsoft.com/library/92x05xfs.aspx) elemento, imediatamente abaixo de abertura **projeto** marca.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


O **importação** elemento é usado para importar o conteúdo de outro arquivo de projeto do MSBuild para o arquivo de projeto MSBuild atual. Nesse caso, o **TargetEnvPropsFile** parâmetro fornece o nome do arquivo do arquivo de projeto que você deseja importar. Você pode fornecer um valor para esse parâmetro quando você executar o MSBuild.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Isso efetivamente mescla o conteúdo dos dois arquivos em um único arquivo de projeto. Usando essa abordagem, você pode criar um arquivo de projeto que contém sua configuração de compilação universal e vários arquivos de projeto suplementar que contém propriedades específicas do ambiente. Como resultado, simplesmente executar um comando com um valor de parâmetro diferente permite que você implantar sua solução para um ambiente diferente.

![](understanding-the-project-file/_static/image3.png)

Dividir seus arquivos de projeto dessa maneira é uma boa prática a seguir. Ele permite que os desenvolvedores implantem em vários ambientes, executando um único comando, enquanto evita a duplicação de propriedades de build universal em vários arquivos de projeto.

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto de ambiente específicas para seus próprios ambientes de servidor, consulte [configurando propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma introdução geral para arquivos de projeto do MSBuild e explicou como você pode criar seus próprios arquivos de projeto personalizados para controlar o processo de compilação. Ele também introduziu o conceito de dividir os arquivos de projeto em instruções de compilação universal e propriedades específicas do ambiente de compilação, para torná-lo mais fácil compilar e implantar projetos para vários destinos.

Próximo tópico [Noções básicas sobre o processo de Build](understanding-the-build-process.md), fornece mais informações sobre como você pode usar arquivos de projeto para controle de compilação e implantação ao guiá-lo por meio da implantação de uma solução com um nível de complexidade de realista.

## <a name="further-reading"></a>Leitura adicional

Para obter uma introdução mais detalhada dos arquivos de projeto e o WPP, consulte [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](setting-up-the-contact-manager-solution.md)
> [Próximo](understanding-the-build-process.md)
