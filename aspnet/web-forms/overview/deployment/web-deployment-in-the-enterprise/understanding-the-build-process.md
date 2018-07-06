---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Noções básicas sobre o processo de Build | Microsoft Docs
author: jrjlee
description: Este tópico fornece um passo a passo de um processo de compilação e implantação de escala empresarial. A abordagem descrita neste tópico usa Engin personalizado do Microsoft Build...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 581b7e996bf5aa4c76a6bf3d55a758677c0bf897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804310"
---
<a name="understanding-the-build-process"></a>Noções básicas sobre o processo de compilação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico fornece um passo a passo de um processo de compilação e implantação de escala empresarial. A abordagem descrita neste tópico usa arquivos de projeto personalizados do Microsoft Build Engine (MSBuild) para fornecer controle refinado sobre todos os aspectos do processo. Nos arquivos de projeto, os destinos do MSBuild personalizados são usados para executar utilitários de implantação, como a ferramenta de implantação da Web do Internet Information Services (IIS) (MSDeploy.exe) e o utilitário de implantação de banco de dados VSDBCMD.exe.
> 
> > [!NOTE]
> > O tópico anterior [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), descritos os principais componentes de um arquivo de projeto do MSBuild e introduziu o conceito de divisão de arquivos de projeto para dar suporte à implantação em vários ambientes de destino. Se você não ainda estiver familiarizado com esses conceitos, leia [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) antes de trabalhar por meio deste tópico.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="build-and-deployment-overview"></a>Compilação e visão geral da implantação

No [entre em contato com o Gerenciador soluções](the-contact-manager-solution.md), três arquivos de controlam o processo de compilação e implantação:

- Um *arquivo de projeto universal* (*Publish.proj*). Isso contém instruções de compilação e implantação que não são alterados entre ambientes de destino.
- Uma *arquivo de projeto específicas do ambiente* (*Env Dev.proj*). Isso contém as configurações de compilação e implantação que são específicas para um ambiente de destino específico. Por exemplo, você pode usar o *Env Dev.proj* arquivo para fornecer configurações para um ambiente de teste ou de desenvolvedor e criar um arquivo alternativo nomeado *Stage.proj Env* para fornecer configurações para um preparo ambiente.
- Um *arquivo de comando* (*Publish-Dev.cmd*). Isso contém um MSBuild.exe comando que especifica qual projeto arquivos que você deseja executar. Você pode criar um arquivo de comando para cada ambiente de destino, em que cada arquivo contém um comando MSBuild.exe que especifica um arquivo de projeto diferente do ambiente específico. Isso permite ao desenvolvedor implantar em ambientes diferentes, executando o arquivo de comando apropriado.

Na solução de exemplo, você pode encontrar esses três arquivos na pasta da solução de publicação.

![](understanding-the-build-process/_static/image1.png)

Antes de examinar esses arquivos em mais detalhes, vamos dar uma olhada em como o processo geral de compilação funciona quando você usa essa abordagem. Em um alto nível, o processo de compilação e implantação tem esta aparência:

![](understanding-the-build-process/_static/image2.png)

A primeira coisa que acontece é que os dois arquivos de projeto&#x2014;que contém instruções de compilação e implantação universais e uma que contém configurações específicas do ambiente&#x2014;são mesclados em um único arquivo de projeto. MSBuild funciona, em seguida, as instruções no arquivo de projeto. Ele cria cada um dos projetos na solução, usando o arquivo de projeto para cada projeto. Ele, em seguida, chama o utilitário VSDBCMD para implantar o conteúdo da web e bancos de dados para o ambiente de destino e de outras ferramentas, como (MSDeploy.exe) de implantação da Web.

Do início ao fim, o processo de compilação e implantação executa as seguintes tarefas:

1. Ele exclui o conteúdo do diretório de saída, em preparação para uma nova compilação.
2. Ele compila cada projeto na solução:

    1. Para projetos da web&#x2014;nesse caso, um aplicativo web ASP.NET MVC e um WCF web service&#x2014;o processo de compilação cria um pacote de implantação da web para cada projeto.
    2. Para projetos de banco de dados, o processo de compilação cria um manifesto de implantação (arquivo .deploymanifest) para cada projeto.
3. Ele usa o utilitário VSDBCMD.exe para implantar cada projeto na solução, usando várias propriedades dos arquivos de projeto de banco de dados&#x2014;uma cadeia de caracteres de conexão de destino e um nome de banco de dados&#x2014;junto com o arquivo .deploymanifest.
4. Ele usa o utilitário MSDeploy.exe para implantar cada projeto da web na solução, usando várias propriedades dos arquivos de projeto para controlar o processo de implantação.

Você pode usar a solução de exemplo para rastrear esse processo em mais detalhes.

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto de ambiente específicas para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Invocar o processo de implantação e compilação

Para implantar a solução de Gerenciador de contatos em um ambiente de teste do desenvolvedor, o desenvolvedor é executado o *Dev.cmd publicar* arquivo de comando. Isso invoca MSBuild.exe, especificando *Publish.proj* como o arquivo de projeto para executar e *Dev.proj Env* como um valor de parâmetro.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> O **/fl** alternar (abreviação de **/fileLogger**) registra a saída da compilação em um arquivo denominado *MSBuild* no diretório atual. Para obter mais informações, consulte o [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


Neste ponto, o MSBuild começa a ser executado, carrega o *Publish.proj* arquivo e começará a processar as instruções dentro dele. A primeira instrução informa ao MSBuild para importar o projeto de arquivos que o **TargetEnvPropsFile** parâmetro especifica.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


O **TargetEnvPropsFile** parâmetro especifica o *Env Dev.proj* arquivo, para que o MSBuild mescla o conteúdo da *Env Dev.proj* o arquivo no  *Publish.proj* arquivo.

Os próximos elementos MSBuild encontra no arquivo de projeto mescladas são grupos de propriedade. Propriedades são processadas na ordem em que aparecem no arquivo. MSBuild cria um par chave-valor para cada propriedade, fornecendo que quaisquer condições especificadas são atendidas. As propriedades definidas posteriormente no arquivo substituirá todas as propriedades com o mesmo nome definido anteriormente no arquivo. Por exemplo, considere a **OutputRoot** propriedades.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Quando o MSBuild processa o primeiro **OutputRoot** elemento, fornecendo um parâmetro nomeado da mesma forma não foi fornecido, ele define o valor da **OutputRoot** propriedade **... \Publish\Out**. Quando ele encontra o segundo **OutputRoot** elemento, se a condição for avaliada como **verdadeiro**, ele substituirá o valor da **OutputRoot** propriedade com o valor da **OutDir** parâmetro.

O próximo elemento MSBuild encontra é um grupo único item, que contém um item denominado **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild processa essa instrução, criando uma lista de itens chamada **ProjectsToBuild**. Nesse caso, a lista de itens contiver um único valor&#x2014;o caminho e o nome do arquivo de solução.

Neste ponto, os elementos restantes são alvos. Destinos são processados de forma diferente de propriedades e itens de&#x2014;essencialmente, destinos não são processados, a menos que eles explicitamente são especificados pelo usuário ou invocados por outra construção dentro do arquivo de projeto. Lembre-se de que a abertura **Project** marca inclui um **DefaultTargets** atributo.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Isso instrui o MSBuild para invocar o **FullPublish** de destino, se os destinos não são especificado quando MSBuild.exe é invocado. O **FullPublish** destino não contém quaisquer tarefas; em vez disso, ele simplesmente especifica uma lista de dependências.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Essa dependência informa ao MSBuild que a fim de executar o **FullPublish** destino, ele precisa chamar essa lista de destinos na ordem fornecida:

1. Ele deve invocar o **Clean** destino.
2. Ele deve invocar o **BuildProjects** destino.
3. Ele deve invocar o **GatherPackagesForPublishing** destino.
4. Ele deve invocar o **PublishDbPackages** destino.
5. Ele deve invocar o **PublishWebPackages** destino.

### <a name="the-clean-target"></a>O destino limpar

O **Clean** destino basicamente exclui o diretório de saída e todo seu conteúdo, como preparação para uma nova compilação.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Observe que o destino inclui um **ItemGroup** elemento. Quando você define propriedades ou itens dentro de um **destino** elemento, você está criando *dinâmico* itens e propriedades. Em outras palavras, as propriedades ou itens não são processados até que o destino é executado. O diretório de saída talvez não existe ou contém arquivos de até que o processo de build é iniciado, portanto, não é possível criar o  **\_FilesToDelete** lista como um item estático; você precisa esperar até que a execução está em andamento. Como tal, você deve criar a lista como um item dinâmico no destino.

> [!NOTE]
> Nesse caso, porque o **Clean** destino seja o primeiro a ser executado, não é necessário usar um grupo de item dinâmico real. No entanto, é recomendável usar as propriedades dinâmicas e itens nesse tipo de cenário, como você talvez queira executar destinos em uma ordem diferente em algum momento.  
> Você também deve ter como objetivo evitar declarar itens que nunca serão usados. Se você tiver itens que serão usados apenas para um destino específico, considere colocá-los dentro do destino para remover qualquer sobrecarga desnecessária no processo de compilação.


Dynamic itens à parte, o **Clean** destino é bastante simples e utiliza o interno **mensagem**, **excluir**, e **RemoveDir**tarefas para:

1. Envie uma mensagem para o agente de log.
2. Crie uma lista de arquivos a serem excluídos.
3. Exclua os arquivos.
4. Remova o diretório de saída.

### <a name="the-buildprojects-target"></a>O destino BuildProjects

O **BuildProjects** destino basicamente cria todos os projetos na solução de exemplo.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Esse destino foi descrito em detalhes no tópico anterior [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), para ilustrar como destinos e tarefas referenciarem itens e propriedades. Neste ponto, você está interessado principalmente a **MSBuild** tarefa. Você pode usar essa tarefa para criar vários projetos. A tarefa não cria uma nova instância da MSBuild.exe; Ele usa a instância em execução atual para compilar cada projeto. Os pontos principais de interesse neste exemplo são as propriedades de implantação:

- O **DeployOnBuild** propriedade instrui o MSBuild a executar quaisquer instruções de implantação nas configurações do projeto quando a compilação de cada projeto estiver concluída.
- O **DeployTarget** propriedade identifica o destino que você deseja invocar depois que o projeto é compilado. Nesse caso, o **pacote** destino cria a saída do projeto em um pacote implantável de web.

> [!NOTE]
> O **pacote** destino invoca o Web Publishing Pipeline (WPP), que fornece a integração entre o MSBuild e a implantação da Web. Se você quiser dar uma olhada os destinos internos que fornece o WPP, examine os *Microsoft.Web.Publishing.targets* arquivo na pasta % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>O destino GatherPackagesForPublishing

Se você estude o **GatherPackagesForPublishing** destino, você observará que ele realmente não contém quaisquer tarefas. Em vez disso, ele contém um grupo de item único que define três itens dinâmicos.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Esses itens se referem aos pacotes de implantação que foram criados quando o **BuildProjects** destino foi executado. Você não foi possível definir esses itens estaticamente no arquivo de projeto, pois os arquivos ao qual se referem os itens não existem até que o **BuildProjects** destino é executado. Em vez disso, os itens devem ser definidos dinamicamente dentro de um destino que não será chamado até depois que o **BuildProjects** destino é executado.

Os itens não são usados dentro desse destino&#x2014;este destino simplesmente cria os itens e os metadados associados a cada valor do item. Depois que esses elementos são processados, o **PublishPackages** item conterá dois valores, o caminho para o *ContactManager.Mvc.deploy.cmd* arquivo e o caminho para o  *ContactManager.Service.deploy.cmd* arquivo. A implantação da Web cria esses arquivos como parte do pacote da web para cada projeto, e esses são os arquivos que você deve chamar no servidor de destino para implantar os pacotes. Se você abrir um desses arquivos, basicamente, você verá um comando MSDeploy.exe com vários valores de parâmetro de compilação específica.

O **DbPublishPackages** item conterá um único valor, o caminho para o *ContactManager.Database.deploymanifest* arquivo.

> [!NOTE]
> Um arquivo .deploymanifest é gerado quando você compila um projeto de banco de dados e usa o mesmo esquema como um arquivo de projeto MSBuild. Ele contém todas as informações necessárias para implantar um banco de dados, incluindo o local do esquema de banco de dados (.dbschema) e detalhes de quaisquer scripts de pré-implantação e pós-implantação. Para obter mais informações, consulte [visão geral de um de banco de dados de compilação e implantação do](https://msdn.microsoft.com/library/aa833165.aspx).


Você aprenderá mais sobre como pacotes de implantação e manifestos de implantação de banco de dados são criados e usados no [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md) e [Implantando projetos de banco de dados](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>O destino PublishDbPackages

Falando resumidamente, o **PublishDbPackages** destino invoca o utilitário VSDBCMD para implantar o **ContactManager** banco de dados para um ambiente de destino. Configurando a implantação de banco de dados envolve muitas decisões e nuances, e você aprenderá mais sobre isso em [implantar projetos de banco de dados](deploying-database-projects.md) e [Personalizando as implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Neste tópico, vamos nos concentrar em como esse destino realmente funciona.

Primeiro, observe que a marca de abertura inclui um **saídas** atributo.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Este é um exemplo da *envio em lote de destino*. Em arquivos de projeto do MSBuild, o envio em lote é uma técnica para iterar em coleções. O valor da **saídas** atributo, **"% (DbPublishPackages.Identity)"**, refere-se ao **identidade** propriedade de metadados do **DbPublishPackages**  lista de itens. Essa notação **Outputs=%***(ItemList.ItemMetadataName)*, é convertido como:

- Dividir os itens na **DbPublishPackages** em lotes de itens que contêm os mesmos **identidade** valor de metadados.
- Execute o destino de uma vez por lote.

> [!NOTE]
> **Identidade** é um dos [os valores de metadados internos](https://msdn.microsoft.com/library/ms164313.aspx) que é atribuído a cada item na criação. Refere-se ao valor da **Include** atributo na **Item** elemento&#x2014;em outras palavras, o caminho e o nome do item.


Nesse caso, porque nunca deverá haver mais de um item com o mesmo caminho e nome de arquivo, essencialmente, trabalhamos com tamanhos de lote de um. O destino é executado uma vez para cada pacote de banco de dados.

Você pode ver uma notação semelhante na  **\_Cmd** propriedade, que cria um comando VSDBCMD com as opções apropriadas.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


Nesse caso, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, e **%(DbPublishPackages.FullPath)** todas se referem a valores de metadados do **DbPublishPackages** coleção de itens. O  **\_Cmd** propriedade é usada pelo **Exec** tarefa, que chama o comando.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Como resultado dessa notação, o **Exec** tarefa será criar lotes com base em combinações exclusivas da **DatabaseConnectionString**, **TargetDatabase**e **FullPath** valores de metadados e a tarefa serão executada uma vez para cada lote. Este é um exemplo da *lote de tarefas*. No entanto, porque o nível de destino envio em lote já foi dividido nossa coleção de itens em lotes de item único, o **Exec** tarefa será executada apenas uma vez para cada iteração do destino. Em outras palavras, essa tarefa invoca o utilitário VSDBCMD uma vez para cada pacote de banco de dados na solução.

> [!NOTE]
> Para obter mais informações sobre o destino e o lote de tarefas, consulte MSBuild [envio em lote](https://msdn.microsoft.com/library/ms171473.aspx), [metadados do Item no lote de destino](https://msdn.microsoft.com/library/ms228229.aspx), e [metadados do Item no lote de tarefas](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>O destino PublishWebPackages

Nesse ponto, você invocou a **BuildProjects** destino, que gera um pacote de implantação da web para cada projeto na solução de exemplo. Que acompanha cada pacote é um *Deploy. cmd* arquivo, que contém os comandos de MSDeploy.exe necessários para implantar o pacote para o ambiente de destino, e uma *SetParameters.xml* arquivo, que especifica o detalhes necessários do ambiente de destino. Você invocou também o **GatherPackagesForPublishing** destino, que gera uma coleção de item que contém o *Deploy. cmd* arquivos que você está interessado. Essencialmente, o **PublishWebPackages** destino realiza as seguintes funções:

- Ela manipula a *SetParameters.xml* arquivo para cada pacote incluir os detalhes corretos para o ambiente de destino, usando o **XmlPoke** tarefa.
- Ele invoca o *Deploy. cmd* arquivo para cada pacote, usando as opções apropriadas.

Assim como o **PublishDbPackages** destino, o **PublishWebPackages** destino usa o envio em lote de destino para garantir que o destino seja executado uma vez para cada pacote de web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Dentro do destino, o **Exec** tarefa é usada para executar o *Deploy. cmd* arquivo para cada pacote da web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Para obter mais informações sobre como configurar a implantação de pacotes da web, consulte [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu um passo a passo de como os arquivos de projeto de divisão são usados para controlar o processo de compilação e implantação do início ao fim para a solução de exemplo do Gerenciador de contatos. Usando essa abordagem permite que você execute complexas, implantações de grande porte em uma etapa única, repetível, executando um arquivo de comando específicas do ambiente.

## <a name="further-reading"></a>Leitura adicional

Para obter uma introdução mais detalhada dos arquivos de projeto e o WPP, consulte [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-project-file.md)
> [Próximo](building-and-packaging-web-application-projects.md)
