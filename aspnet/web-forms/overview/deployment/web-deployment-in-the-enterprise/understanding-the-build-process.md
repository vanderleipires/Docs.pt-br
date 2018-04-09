---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Noções básicas sobre o processo de compilação | Microsoft Docs
author: jrjlee
description: Este tópico fornece um passo a passo de um processo de compilação e implantação de grande porte. A abordagem descrita neste tópico usa Engin de compilação personalizada de Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 4544a5e6212ea9b1247062dc35edc135ff7ca354
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="understanding-the-build-process"></a>Noções básicas sobre o processo de compilação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico fornece um passo a passo de um processo de compilação e implantação de grande porte. A abordagem descrita neste tópico usa arquivos de projeto Microsoft Build Engine (MSBuild) personalizados para fornecer controle refinado sobre todos os aspectos do processo. Nos arquivos de projeto, destinos do MSBuild personalizados são usados para executar o utilitário de implantação como a ferramenta de implantação da Web do Internet Information Services (IIS) (MSDeploy.exe) e o utilitário de implantação de banco de dados VSDBCMD.exe.
> 
> > [!NOTE]
> > O tópico anterior, [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), descritos os principais componentes de um arquivo de projeto MSBuild e introduziu o conceito de divisão de arquivos de projeto para dar suporte à implantação em vários ambientes de destino. Se você não ainda estiver familiarizado com esses conceitos, leia [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) antes de trabalhar por meio deste tópico.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="build-and-deployment-overview"></a>Compilação e visão geral da implantação

No [Contact Manager solução](the-contact-manager-solution.md), três arquivos de controlam o processo de compilação e implantação:

- Um *arquivo de projeto universal* (*Publish.proj*). Contém instruções de compilação e implantação que não são alterados entre os ambientes de destino.
- Um *arquivo de projeto específico do ambiente* (*Dev.proj Env*). Contém as configurações de compilação e implantação que são específicas para um ambiente de destino específico. Por exemplo, você pode usar o *Env-Dev.proj* arquivo para fornecer configurações para um ambiente de teste ou de desenvolvedor e crie um arquivo alternativo chamado *Stage.proj Env* para fornecer configurações para um teste ambiente.
- Um *arquivo de comando* (*Dev.cmd publicar*). Ele contém um MSBuild.exe comando que especifica qual projeto arquivos que você deseja executar. Você pode criar um arquivo de comando para cada ambiente de destino, onde cada arquivo contém um comando MSBuild.exe que especifica um arquivo de projeto específico do ambiente diferente. Isso permite que o desenvolvedor implantar em ambientes diferentes executando o arquivo de comando apropriado.

Na solução de exemplo, você pode encontrar esses três arquivos na pasta de solução de publicação.

![](understanding-the-build-process/_static/image1.png)

Antes de examinar esses arquivos em mais detalhes, vamos dar uma olhada em como o processo geral de criação funciona quando você usar essa abordagem. Em um nível alto, o processo de compilação e implantação tem esta aparência:

![](understanding-the-build-process/_static/image2.png)

A primeira coisa que acontece é que os dois arquivos de projeto&#x2014;que contém instruções de compilação e implantação universais e uma que contém configurações específicas do ambiente&#x2014;são mesclados em um único arquivo de projeto. MSBuild funciona, em seguida, as instruções no arquivo de projeto. Ele cria cada um dos projetos na solução, usando o arquivo de projeto para cada projeto. Ela então chama a atenção para outras ferramentas, como implantação de Web (MSDeploy.exe) e o utilitário VSDBCMD para implantar o conteúdo da web e os bancos de dados para o ambiente de destino.

Do início ao fim, o processo de compilação e implantação executa essas tarefas:

1. Ele exclui o conteúdo do diretório de saída, em preparação para uma nova compilação.
2. Ele cria cada projeto na solução:

    1. Para projetos web&#x2014;nesse caso, um aplicativo ASP.NET MVC e um WCF serviço web&#x2014;o processo de compilação cria um pacote de implantação da web para cada projeto.
    2. Para projetos de banco de dados, o processo de compilação cria um manifesto de implantação (arquivo .deploymanifest) para cada projeto.
3. Ele usa o utilitário VSDBCMD.exe para implantar cada projeto de banco de dados na solução, usando várias propriedades de arquivos de projeto&#x2014;uma cadeia de caracteres de conexão de destino e um nome de banco de dados&#x2014;junto com o arquivo .deploymanifest.
4. Ele usa o utilitário MSDeploy.exe para implantar cada projeto da web na solução, usando várias propriedades de arquivos de projeto para controlar o processo de implantação.

Você pode usar a solução de exemplo para este processo em mais detalhes de rastreamento.

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específico de ambiente para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação de um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Invocar o processo de implantação e compilação

Para implantar a solução de Gerenciador de contato para um ambiente de teste do desenvolvedor, o desenvolvedor é executado o *publicar Dev.cmd* arquivo de comando. Isso chama o MSBuild.exe, especificando *Publish.proj* como o arquivo de projeto para executar e *Dev.proj Env* como um valor de parâmetro.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> O **/fl** alternar (abreviação de **/fileLogger**) registra a saída da compilação em um arquivo denominado *msbuild.log* no diretório atual. Para obter mais informações, consulte o [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).


Neste ponto, o MSBuild começa a ser executado, carrega o *Publish.proj* arquivo e começa a processar as instruções dentro dele. A primeira instrução informa MSBuild para importar o projeto de arquivos que o **TargetEnvPropsFile** parâmetro especifica.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


O **TargetEnvPropsFile** parâmetro especifica o *Env-Dev.proj* de arquivo, para que o MSBuild mescla o conteúdo do *Env-Dev.proj* o arquivo no  *Publish.proj* arquivo.

Os próximos elementos MSBuild encontra no arquivo de projeto mescladas são grupos de propriedades. Propriedades são processadas na ordem em que aparecem no arquivo. O MSBuild criará um par chave-valor para cada propriedade, fornecendo que quaisquer condições especificadas são atendidas. Propriedades definidas posteriormente no arquivo substituirá as propriedades com o mesmo nome definido anteriormente no arquivo. Por exemplo, considere o **OutputRoot** propriedades.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Quando o MSBuild processa o primeiro **OutputRoot** elemento, fornecendo um parâmetro nomeado da mesma forma não foi fornecido, ele define o valor da **OutputRoot** propriedade **. \Publish\Out**. Quando ele encontra a segunda **OutputRoot** elemento, se a condição for avaliada como **true**, ele substituirá o valor da **OutputRoot** propriedade com o valor da **OutDir** parâmetro.

O próximo elemento MSBuild encontra é um grupo único item, que contém um item denominado **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild processa esta instrução, criando uma lista de itens denominada **ProjectsToBuild**. Nesse caso, a lista de itens contiver um único valor&#x2014;o caminho e o nome do arquivo de solução.

Neste ponto, os elementos restantes são destinos. Destinos são processados de forma diferente de propriedades e itens&#x2014;essencialmente, destinos não são processados, a menos que eles explicitamente são especificados pelo usuário ou invocados por outra construção dentro do arquivo de projeto. Lembre-se de que a abertura **projeto** marca inclui um **DefaultTargets** atributo.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Isso instrui o MSBuild para invocar o **FullPublish** de destino, se destinos não for especificado quando MSBuild.exe é invocado. O **FullPublish** destino não contém nenhuma tarefa; em vez disso, ele simplesmente especifica uma lista de dependências.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Essa dependência informa o MSBuild em ordem ao executar o **FullPublish** destino, ele precisa chamar essa lista de destinos na ordem fornecida:

1. Ele deve chamar o **limpar** destino.
2. Ele deve chamar o **BuildProjects** destino.
3. Ele deve chamar o **GatherPackagesForPublishing** destino.
4. Ele deve chamar o **PublishDbPackages** destino.
5. Ele deve chamar o **PublishWebPackages** destino.

### <a name="the-clean-target"></a>O destino de limpo

O **limpar** destino basicamente exclui o diretório de saída e todo seu conteúdo, como preparação para uma nova compilação.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Observe que o destino inclui um **ItemGroup** elemento. Quando você define propriedades ou os itens dentro de um **destino** elemento, você está criando *dinâmico* propriedades e itens. Em outras palavras, as propriedades ou os itens não são processados até que o destino seja executado. O diretório de saída pode não existe ou contém arquivos até que o processo de compilação é iniciado, portanto não é possível criar o  **\_FilesToDelete** lista como um item estático; você deve aguardar até que a execução está em andamento. Como tal, você deve criar uma lista como um item dinâmico no destino.

> [!NOTE]
> Nesse caso, porque o **limpar** destino seja o primeiro a ser executado, não é necessário usar um grupo de itens dinâmicos real. No entanto, é recomendável usar propriedades dinâmicas e itens nesse tipo de cenário, que talvez você queira executar destinos em uma ordem diferente em algum momento.  
> Além disso, você deve visar para evitar declarando itens nunca serão usados. Se você tiver itens que serão usados somente por um destino específico, considere colocá-los dentro do destino para remover qualquer sobrecarga desnecessária no processo de compilação.


Dinâmico itens à parte, o **limpar** destino é bastante simples e utiliza o interno **mensagem**, **excluir**, e **RemoveDir**tarefas para:

1. Envie uma mensagem para o agente de log.
2. Crie uma lista de arquivos a serem excluídos.
3. Exclua os arquivos.
4. Remova o diretório de saída.

### <a name="the-buildprojects-target"></a>O destino BuildProjects

O **BuildProjects** destino basicamente cria todos os projetos na solução de exemplo.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Esse destino foi descrito em detalhes no tópico anterior, [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), para ilustrar como destinos e tarefas referenciarem propriedades e itens. Neste ponto, você está interessado principalmente o **MSBuild** tarefa. Você pode usar essa tarefa para criar vários projetos. A tarefa não cria uma nova instância do MSBuild.exe; Ele usa a instância em execução atual para cada projeto de compilação. Os principais pontos de interesse neste exemplo são as propriedades de implantação:

- O **DeployOnBuild** propriedade instrui o MSBuild para executar as instruções de implantação nas configurações do projeto quando a compilação de cada projeto estiver concluída.
- O **DeployTarget** propriedade identifica o destino que você deseja invocar depois que o projeto é compilado. Nesse caso, o **pacote** destino cria a saída do projeto em um pacote de implantação web.

> [!NOTE]
> O **pacote** destino invoca o Web Publishing Pipeline (WPP), que fornece a integração entre o MSBuild e a implantação da Web. Se você quiser examinar os destinos internos que fornece o WPP, examine o *Microsoft.Web.Publishing.targets* arquivo na pasta % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


### <a name="the-gatherpackagesforpublishing-target"></a>O destino GatherPackagesForPublishing

Se você estude o **GatherPackagesForPublishing** destino, você observará que, na verdade, ele não contém nenhuma tarefa. Em vez disso, ele contém um grupo de itens único que define três itens dinâmicos.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Consultem esses itens para os pacotes de implantação que foram criados quando o **BuildProjects** destino foi executado. Não foi possível definir esses itens de estaticamente no arquivo de projeto, pois os arquivos à qual os itens de referem não existem até que o **BuildProjects** destino é executado. Em vez disso, os itens devem ser definidos dinamicamente em um destino que não é invocado até depois que o **BuildProjects** destino é executado.

Os itens não são usados dentro desse destino&#x2014;esse destino simplesmente cria os itens e os metadados associados a cada valor do item. Depois que esses elementos são processados, o **PublishPackages** item conterá dois valores, o caminho para o *ContactManager.Mvc.deploy.cmd* de arquivo e o caminho para o  *ContactManager.Service.deploy.cmd* arquivo. A implantação da Web cria esses arquivos como parte do pacote para cada projeto da web, e estes são os arquivos que você deve chamar no servidor de destino para implantar os pacotes. Se você abrir um desses arquivos, você verá basicamente um comando MSDeploy.exe com vários valores de parâmetro de compilação específica.

O **DbPublishPackages** item conterá um único valor, o caminho para o *ContactManager.Database.deploymanifest* arquivo.

> [!NOTE]
> Um arquivo .deploymanifest é gerado quando você cria um projeto de banco de dados e usa o mesmo esquema como um arquivo de projeto do MSBuild. Ele contém todas as informações necessárias para implantar um banco de dados, incluindo o local do esquema de banco de dados (.dbschema) e detalhes de todos os scripts pré e pós-implantação. Para obter mais informações, consulte [visão geral de uma de compilação de banco de dados e implantação](https://msdn.microsoft.com/library/aa833165.aspx).


Você aprenderá mais sobre como os pacotes de implantação e manifestos de implantação de banco de dados são criados e usados no [criação e a projetos de aplicativo Web de empacotamento](building-and-packaging-web-application-projects.md) e [implantar projetos de banco de dados](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>O destino PublishDbPackages

Brevemente falando, o **PublishDbPackages** destino invoca o utilitário VSDBCMD para implantar o **ContactManager** banco de dados para um ambiente de destino. A configuração de implantação de banco de dados envolve muitas decisões e nuances e você aprenderá mais sobre isso em [implantar projetos de banco de dados](deploying-database-projects.md) e [personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Neste tópico, nos concentraremos em como esse destino realmente funciona.

Primeiro, observe que a marca de abertura inclui um **saídas** atributo.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Este é um exemplo de *o envio em lote de destino*. Em arquivos de projeto do MSBuild, o envio em lote é uma técnica para iteração pelas coleções. O valor da **saídas** atributo, **"% (DbPublishPackages.Identity)"**, refere-se ao **identidade** propriedade de metadados a **DbPublishPackages**  lista de itens. Esta notação **Outputs=%***(ItemList.ItemMetadataName)*, é traduzido como:

- Dividir os itens na **DbPublishPackages** em lotes de itens que contêm o mesmo **identidade** valor de metadados.
- Execute o destino de uma vez por lote.

> [!NOTE]
> **Identidade** é uma da [valores de metadados internas](https://msdn.microsoft.com/library/ms164313.aspx) que é atribuído a cada item na criação. Ele se refere ao valor da **incluir** atributo no **Item** elemento&#x2014;em outras palavras, o caminho e o nome do item.


Nesse caso, porque nunca deverá haver mais de um item com o mesmo caminho e nome de arquivo, estamos essencialmente trabalhando com tamanhos de lote de um. O destino é executado uma vez para cada pacote de banco de dados.

Você pode ver uma notação semelhante no  **\_Cmd** propriedade, que cria um comando VSDBCMD com as opções apropriadas.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


Nesse caso, **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, e **%(DbPublishPackages.FullPath)** todos referir os valores de metadados de **DbPublishPackages** coleção de itens. O  **\_Cmd** propriedade é usada pelo **Exec** tarefa, que chama o comando.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Como resultado dessa notação, o **Exec** tarefa criará lotes com base em combinações exclusivas do **DatabaseConnectionString**, **TargetDatabase**e **FullPath** valores de metadados e a tarefa serão executada uma vez para cada lote. Este é um exemplo de *lote de tarefas*. No entanto, porque o lote de nível de destino já foi dividido nossa coleção de itens em lotes com item único, o **Exec** tarefa será executada apenas uma vez para cada iteração do destino. Em outras palavras, essa tarefa invoca o utilitário VSDBCMD uma vez para cada pacote de banco de dados na solução.

> [!NOTE]
> Para obter mais informações sobre o destino e o lote de tarefas, consulte MSBuild [lote](https://msdn.microsoft.com/library/ms171473.aspx), [metadados de Item no lote de destino](https://msdn.microsoft.com/library/ms228229.aspx), e [metadados de Item no lote de tarefas](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>O destino PublishWebPackages

Neste ponto, você chamou o **BuildProjects** destino, o que gera um pacote de implantação da web para cada projeto na solução de exemplo. Que acompanha cada pacote é um *Deploy* arquivo, que contém os comandos de MSDeploy.exe necessários para implantar o pacote para o ambiente de destino, e um *SetParameters.xml* arquivo, que especifica o detalhes necessários ao ambiente de destino. Você também já chamou o **GatherPackagesForPublishing** destino, o que gera uma coleção de itens que contém o *Deploy* arquivos que você está interessado em. Essencialmente, o **PublishWebPackages** destino realiza as seguintes funções:

- Manipular o *SetParameters.xml* arquivo para cada pacote incluir os detalhes corretos para o ambiente de destino, usando o **XmlPoke** tarefa.
- Ele chama o *Deploy* arquivo para cada pacote, usando as opções apropriadas.

Assim como o **PublishDbPackages** destino, o **PublishWebPackages** destino usa o envio em lote de destino para garantir que o destino seja executado uma vez para cada pacote de web.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


No destino, o **Exec** tarefa é usada para executar o *Deploy* arquivo para cada pacote da web.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Para obter mais informações sobre como configurar a implantação de pacotes da web, consulte [criação e a projetos de aplicativo Web de empacotamento](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu um passo a passo de como os arquivos de projeto de divisão são usados para controlar o processo de compilação e implantação do início ao fim para a solução de exemplo do gerente do contato. Usando essa abordagem permite executar complexo, as implantações de grande porte em uma etapa única, repetível, executando um arquivo de comando específico do ambiente.

## <a name="further-reading"></a>Leitura adicional

Para obter uma introdução mais detalhada dos arquivos de projeto e o WPP, consulte [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://amzn.com/0735645248) Sayed Hashimi de Ibrahim e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-project-file.md)
> [Próximo](building-and-packaging-web-application-projects.md)
