---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: (Criando aplicativos de nuvem do mundo Real com o Azure) do controle de fonte | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5df863762523b62759bb4f7849ca2635e5241b0a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577789"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Controle de origem (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Controle do código-fonte é essencial para todos os projetos de desenvolvimento de nuvem, não apenas a ambientes de equipe. Você jamais pensou que de edição de código-fonte ou até mesmo um documento do Word sem uma função de desfazer e backups automáticos e controle do código-fonte fornece essas funções em um nível de projeto em que eles podem economizar ainda mais tempo quando algo dá errado. Com os serviços de controle do código-fonte de nuvem, você não precisa se preocupar sobre a configuração complexa e você pode usar o controle de origem de repositórios do Azure gratuita para até 5 usuários.

A primeira parte deste capítulo explica os três principais práticas recomendadas para ter em mente:

- [Trate os scripts de automação como código-fonte](#scripts) e versão-los junto com o código do aplicativo.
- [Nunca fazer check-in segredos](#secrets) (dados confidenciais, como credenciais) em um repositório de código-fonte.
- [Configurar ramificações do código-fonte](#devops) para habilitar o fluxo de trabalho de DevOps.

O restante do capítulo fornece algumas implementações de exemplo desses padrões no Visual Studio, o Azure e os repositórios do Azure:

- [Adicionar scripts ao controle do código-fonte no Visual Studio](#vsscripts)
- [Dados confidenciais no Azure Store](#appsettings)
- [Use o Git no Visual Studio e repositórios do Azure](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Trate os scripts de automação como código-fonte

Quando você estiver trabalhando em um projeto de nuvem, você está alterando as coisas com frequência e você deseja ser capaz de reagir rapidamente aos problemas relatados por seus clientes. Responder rapidamente envolve o uso de scripts de automação, conforme explicado a [automatizar tudo](automate-everything.md) capítulo. Todos os scripts que você usa para criar seu ambiente, implante a ele, ele, etc., precisam ser sincronizados com o código de origem do aplicativo de escala.

Para manter os scripts em sincronia com o código, armazená-las em seu sistema de controle de origem. Em seguida, se você precisar reverter as alterações ou fazer uma correção rápida para o código de produção que é diferente do código de desenvolvimento, não precisará perder tempo tentando rastrear quais configurações foram alteradas ou quais membros da equipe tenham cópias da versão que você precisa. Você tem a garantia de que os scripts que você precisa estão em sincronizado com a base de código que você precisa deles para, e você tem a garantia que todos os membros da equipe estão trabalhando com os mesmos scripts. Em seguida, se você precisar automatizar o teste e implantação de um hot fix de produção ou desenvolvimento de novos recursos, você terá o script certo para o código que precisa ser atualizado.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Não fazer check-in de segredos

Um repositório de código-fonte é normalmente acessível para muitas pessoas para que ele seja um local seguro adequadamente para dados confidenciais como senhas. Se os scripts contam com segredos, como senhas, parametrize essas configurações para que eles não será salvo no código-fonte e armazenam seus segredos em outro lugar.

Por exemplo, o Azure permite que você baixe os arquivos que contêm configurações de publicação para automatizar a criação de perfis de publicação. Esses arquivos incluem nomes de usuário e senhas estão autorizadas a gerenciar seus serviços do Azure. Se você usar esse método para criar perfis de publicação, e se você fazer o check-in desses arquivos ao controle do código-fonte, qualquer pessoa com acesso ao seu repositório pode ver esses nomes de usuário e senhas. Você pode armazenar com segurança a senha no próprio perfil de publicação porque ela é criptografada e está em um *. pubxml* arquivo que não está incluído no controle de origem por padrão.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Ramificações de código-fonte da estrutura para facilitar o fluxo de trabalho de DevOps

Como implementar ramificações no repositório afeta sua capacidade de desenvolver novos recursos e corrigir problemas em produção. Este é um padrão que muita médio porte as equipes usam:

![Estrutura de ramificação de origem](source-control/_static/image1.png)

O branch mestre sempre corresponde ao código que está em produção. Ramificações abaixo mestre correspondem aos diferentes fases no ciclo de vida de desenvolvimento. A ramificação desenvolvimento é onde você implementa novos recursos. Para uma equipe pequena talvez você tenha apenas mestre e desenvolvimento, mas geralmente é recomendável que as pessoas têm uma ramificação de preparo entre desenvolvimento e mestre. Você pode usar preparo para testes de integração final antes que uma atualização seja movida para a produção.

Para equipes grandes pode haver ramificações separadas para cada novo recurso; para uma equipe menor, você pode ter todos os check-in para a ramificação de desenvolvimento.

Se você tiver uma ramificação para cada recurso, quando o recurso r está pronto você mesclar suas alterações de código fonte-se no desenvolvimento ramificar e para baixo em ramificações do recurso. Esse código de origem no processo de mesclagem pode ser demorado, e para evitar esse trabalho e manter os recursos separados, algumas equipes de implementam uma alternativa chamada *[alternâncias de funcionalidades](http://en.wikipedia.org/wiki/Feature_toggle)* (também conhecido como *sinalizadores de recurso*). Isso significa que todo o código para todos os recursos é na mesma ramificação, mas você habilitar ou desabilitar cada recurso, usando as opções no código. Por exemplo, suponha que o recurso A é um novo campo para tarefas de aplicativo Fix It e recurso B adiciona a funcionalidade de cache. O código para os dois recursos pode ser na ramificação desenvolvimento, mas o aplicativo somente exibirá o novo campo quando uma variável é definida como true e ele usará apenas o cache quando uma variável diferente é definida como true. Se o recurso A não está pronto para ser promovido, mas o recurso B estiver pronto, você pode promover todo o código para a produção com a opção de recurso A off e ative o recurso B. Você pode, em seguida, concluir um recurso e promovê-lo mais tarde, tudo com mesclagem de código não código-fonte.

Se você usar ramificações ou alterna para recursos, uma estrutura de ramificação, como isso permite que você fluxo seu código de desenvolvimento para a produção de forma ágil e repetível.

Essa estrutura também permite que você reaja rapidamente aos comentários dos clientes. Se você precisar fazer uma correção rápida para a produção, você também pode fazer que com eficiência de forma ágil. Você pode criar uma ramificação do mestre ou de preparo, e quando ele estiver pronto mesclá-lo para cima no mestre e para baixo em ramificações de desenvolvimento e recursos.

![Branch do hotfix](source-control/_static/image2.png)

Sem uma estrutura de ramificação assim com sua separação das ramificações de desenvolvimento e produção, um problema de produção pode colocá-lo na posição de ter que promover o novo código de recurso, juntamente com sua correção de produção. O novo código de recurso pode não ser totalmente testada e pronta para produção e você talvez precise fazer muito trabalho fazendo as alterações que não estão prontas. Ou, talvez seja necessário atrasar a correção para testar as alterações e prepará-los implantar.

Em seguida, você verá exemplos de como implementar esses três padrões no Visual Studio, o Azure e os repositórios do Azure. Estes são exemplos, em vez de instruções passo a passo de how-to--it detalhadas; Para obter instruções detalhadas que oferecem todos o contexto necessário, consulte a [recursos](#resources) seção no final deste capítulo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Adicionar scripts ao controle do código-fonte no Visual Studio

Você pode adicionar scripts ao controle do código-fonte no Visual Studio, incluindo-os em uma pasta de solução do Visual Studio (supondo que seu projeto está no controle de origem). Aqui está uma maneira de fazê-lo.

Crie uma pasta para os scripts na pasta da solução (na mesma pasta que tem seu *. sln* arquivo).

![Pasta de automação](source-control/_static/image3.png)

Copie os arquivos de script para a pasta.

![Conteúdo da pasta de automação](source-control/_static/image4.png)

No Visual Studio, adicione uma pasta de solução para o projeto.

![Nova seleção de menu da pasta de solução](source-control/_static/image5.png)

E adicione os arquivos de script para a pasta de solução.

![Adicionar a seleção de menu do Item existente](source-control/_static/image6.png)

![Adicionar item existente](source-control/_static/image7.png)

Os arquivos de script agora estão incluídos em seu projeto e controle do código-fonte está controlando suas alterações de versão junto com as alterações de código de origem correspondentes.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Dados confidenciais no Azure Store

Se você executar o aplicativo em um Site da Web do Azure, uma maneira para evitar armazenar credenciais no controle de origem é armazená-las no Azure.

Por exemplo, o aplicativo Fix It armazena no seus Web. config arquivo duas conexão cadeias de caracteres que terão senhas em produção e uma chave que oferece acesso à sua conta de armazenamento do Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Se você colocar valores de produção real para essas configurações no seu *Web. config* arquivo, ou se você colocá-los *Release* arquivo para configurar uma transformação de Web. config para inseri-los durante a implantação, eles serão armazenados no repositório de origem. Se você inserir as cadeias de caracteres de conexão de banco de dados para a produção perfil de publicação, a senha estará em sua *. pubxml* arquivo. (Você pode excluir o *. pubxml* de arquivo de controle de origem, mas, em seguida, você perderá o benefício de compartilhamento de todas as outras configurações de implantação.)

O Azure oferece uma alternativa para o **appSettings** e seções de cadeias de caracteres de conexão do *Web. config* arquivo. Aqui está a parte relevante do **configuração** guia para um site da web no portal de gerenciamento do Azure:

![appSettings e connectionStrings no portal](source-control/_static/image8.png)

Quando você implanta um projeto para este site da web e o aplicativo é executado, quaisquer valores armazenados no Azure substituem valores que estão no arquivo Web. config.

Você pode definir esses valores no Azure usando o portal de gerenciamento ou scripts. O script de automação de criação do ambiente você viu na [automatizar tudo](automate-everything.md) capítulo cria um banco de dados do SQL Azure, obtém o armazenamento e cadeias de caracteres de conexão de banco de dados SQL e armazena esses segredos nas configurações de seu site da web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Observe que os scripts são parametrizados, de modo que os valores reais não são persistidos para o repositório de origem.

Quando você executa localmente no seu ambiente de desenvolvimento, o aplicativo lê o arquivo Web. config local e sua conexão pontos de cadeia de caracteres para um banco de dados SQL Server de LocalDB a *App\_dados* pasta do seu projeto web. Quando você executar o aplicativo no Azure e o aplicativo tenta ler esses valores no arquivo Web. config, o que ele obtém de volta e usa são os valores armazenados para o Site da Web, não o que é, na verdade, no arquivo Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Use o Git no Visual Studio e DevOps do Azure

Você pode usar qualquer ambiente de controle do código-fonte para implementar a estrutura de ramificação do DevOps apresentada anteriormente. Para equipes distribuídas uma [sistema de controle de versão distribuído](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) pode funcionar melhor; para outras equipes um [centralizado sistema](http://en.wikipedia.org/wiki/Revision_control) podem funcionar melhor.

[Git](http://git-scm.com/) é um sistema de controle de versão distribuído popularmente. Quando você usa Git para controle de origem, você tem uma cópia completa do repositório com todo o histórico no computador local. Muitas pessoas preferem que porque é mais fácil continuar trabalhando quando não estiver conectado à rede – você pode continuar a fazer confirmações e reversões, criar e alternar os branches e assim por diante. Mesmo quando você estiver conectado à rede, é mais fácil e rápido para criar ramificações e alternar os branches quando tudo é local. Você também pode fazer confirmações locais e reversões sem causar impacto em outros desenvolvedores. E você pode criar lotes de confirmações antes de enviá-los para o servidor.

[Repositórios do Azure](/azure/devops/repos/index?view=vsts) oferece ambos [Git](/azure/devops/repos/git/?view=vsts) e [Team Foundation Version Control](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; controle do código-fonte centralizado). Introdução ao Azure DevOps [aqui](https://app.vsaex.visualstudio.com/signup).

Visual Studio 2017 inclui interno, primeira classe [suporte ao Git](https://msdn.microsoft.com/library/hh850437.aspx). Aqui está uma rápida demonstração de como isso funciona.

Com um projeto aberto no Visual Studio, clique com botão direito na solução **Gerenciador de soluções**e, em seguida, escolha **adicionar solução ao controle do código-fonte**.

![Adicionar solução ao controle do código-fonte](source-control/_static/image9.png)

Visual Studio pergunta se você deseja usar o TFVC (controle de versão centralizado) ou Git.

![Escolha o controle de origem](source-control/_static/image10.png)

Quando você selecionar o Git e clique em **Okey**, o Visual Studio cria um novo repositório Git local na pasta da solução. O novo repositório não tem arquivos ainda; Você precisa adicioná-los para o repositório fazendo uma confirmação do Git. A solução no botão direito do mouse **Gerenciador de soluções**e, em seguida, clique em **confirmar**.

![Confirmação](source-control/_static/image11.png)

Visual Studio automaticamente prepara todos os arquivos de projeto para a confirmação e listá-los no **Team Explorer** na **alterações incluídas** painel. (Se houver alguns você não deseja incluir na confirmação, você pode selecioná-las, clique com botão direito e clique em **excluir**.)

![Team Explorer](source-control/_static/image12.png)

Insira um comentário de confirmação e clique em **confirmação**, e o Visual Studio executa a confirmação e exibe a ID de confirmação.

![Alterações do Team Explorer](source-control/_static/image13.png)

Agora se você alterar o código para que ele seja diferente do que está no repositório, você pode exibir facilmente as diferenças. Clique com botão direito um arquivo que você alterou, selecione **comparado com Unmodified**, e você obterá uma exibição de comparação que mostra as alterações não confirmadas.

![Comparar com não modificado](source-control/_static/image14.png)

![Comparação mostrando alterações](source-control/_static/image15.png)

Você pode ver facilmente as alterações que você está fazendo e check-in.

Suponha que você precisa fazer uma ramificação – você pode fazer isso no Visual Studio. Na **Team Explorer**, clique em **nova ramificação**.

![Nova ramificação do Team Explorer](source-control/_static/image16.png)

Insira um nome de branch, clique em **criar ramificação**, e se você selecionou **branch de check-out**, Visual Studio automaticamente faz check-out do novo branch.

![Nova ramificação do Team Explorer](source-control/_static/image17.png)

Agora você pode fazer alterações nos arquivos e check-in para essa ramificação. E você pode alternar facilmente entre ramificações e o Visual Studio automaticamente os arquivos para aquele que ramificação você fez check-out as sincronizações. Neste exemplo de página da web título na  *\_layout. cshtml* foi alterado para "Hot Fix 1" no HotFix1 branch.

![Branch Hotfix1](source-control/_static/image18.png)

Se você alternar de volta para o mestre de ramificação, o conteúdo do  *\_layout. cshtml* arquivo reverter automaticamente para que estão no branch mestre.

![Branch mestre](source-control/_static/image19.png)

Esse um exemplo simples de como você pode criar uma ramificação e e alternar entre branches rapidamente. Esse recurso permite que um fluxo de trabalho altamente agile usando a estrutura de ramificação e scripts de automação é apresentado na [automatizar tudo](automate-everything.md) capítulo. Por exemplo, você pode estar trabalhando na ramificação desenvolvimento, criar uma ramificação de hot fix a partir do mestre, alterne para o novo branch, faça as alterações lá e confirmá-las e alterne de volta para a ramificação de desenvolvimento e continuar que estava fazendo.

O que você viu aqui é como você trabalha com um repositório Git local no Visual Studio. Em um ambiente de equipe você normalmente também enviar por push as alterações para cima para um repositório comum. Ferramentas do Visual Studio também permitem que você apontar para um repositório Git remoto. Você pode usar GitHub.com para essa finalidade, ou você pode usar [Git e repositórios Azure](/azure/devops/repos/git/overview?view=vsts) integrado com todos os outros recursos de DevOps do Azure, como o item de trabalho e acompanhamento de bugs.

Isso não é a única maneira que você pode implementar uma estratégia de ramificação do agile, é claro. Você pode habilitar o mesmo fluxo de trabalho agile usando um repositório de controle de fonte centralizada.

## <a name="summary"></a>Resumo

Medir o sucesso do seu sistema de controle do código-fonte com base em quão rapidamente, você pode fazer uma alteração e obtê-lo ao vivo de uma maneira segura e previsível. Se você perceber que está com medo de fazer uma alteração porque você precisa fazer um ou dois dias de teste manual nele, você pode se perguntar o que você precisa fazer process-wise ou test-wise para que você possa fazer essa alteração em minutos ou em pior não há mais de uma hora. Uma estratégia para fazer isso é implementar a integração contínua e entrega contínua, o que abordaremos na [próximo capítulo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações sobre estratégias de expansão, consulte os seguintes recursos:

- [Criando um Pipeline de lançamento com o Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentação do Microsoft Patterns and Practices. Consulte o capítulo 6 para uma discussão sobre estratégias de ramificação. O recurso defensores alterna o ramificações recursos e se ramificações para recursos forem usadas, defende mantê-los e de curta duração (horas ou dias, no máximo).
- [Guia de controle de versão](https://aka.ms/vsarsolutions). Guia para estratégias de ramificação pelo ALM Rangers. Consulte a ramificação Strategies.pdf na guia Downloads.
- [Desenvolvimento de software com alternadores de recursos](https://msdn.microsoft.com/magazine/dn683796.aspx). Artigo da MSDN Magazine.
- [Alternância de recurso](http://martinfowler.com/bliki/FeatureToggle.html). Introdução ao recurso alterna / sinalizadores de recursos no blog de Martin Fowler.
- [Recurso alterna vs ramificações recursos](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Outra postagem do blog sobre os alternadores de recursos, por Dylan Smith.

Para obter mais informações sobre como lidar com informações confidenciais que não devem ser mantidas em repositórios de controle do código-fonte, consulte os seguintes recursos:

- [Práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.NET e o serviço de aplicativo do Azure](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Os Sites do Azure: Como cadeias de caracteres de aplicativo e Conexão cadeias de caracteres de trabalho](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explica o recurso do Azure que substitui `appSettings` e `connectionStrings` dados de *Web. config* arquivo.
- [Personalizar as configurações de aplicativo e nos Sites do Azure - com Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Para obter informações sobre outros métodos para manter a informações confidenciais fora do controle do código-fonte, consulte [ASP.NET MVC: manter privadas as configurações de fora do controle de origem](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Anterior](automate-everything.md)
> [Próximo](continuous-integration-and-continuous-delivery.md)
