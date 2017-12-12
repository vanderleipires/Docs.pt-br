---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Fonte de controle (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: "Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: f244e6bd1cd8abd23b64d07ccafcef5c4db1029b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Controle de origem (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Controle de origem é essencial para todos os projetos de desenvolvimento de nuvem, não apenas a ambientes de equipe. Você não acha de edição de código-fonte ou até mesmo um documento do Word sem uma função de desfazer e backups automáticos e controle de origem fornece essas funções em um nível de projeto em que eles podem economizar ainda mais tempo quando algo dá errado. Com serviços de controle do código-fonte de nuvem, você não precisa se preocupar sobre configuração complexa, e você pode usar o controle de origem Visual Studio Online gratuito para até 5 usuários.

A primeira parte deste capítulo explica três principais práticas recomendadas para ter em mente:

- [Tratar os scripts de automação como código-fonte](#scripts) e versão-los junto com o código do aplicativo.
- [Nunca verificar em segredos](#secrets) (dados confidenciais, como credenciais) em um repositório de código fonte.
- [Configurar as ramificações de origem](#devops) para habilitar o fluxo de trabalho do DevOps.

O restante deste capítulo fornece alguns exemplos de implementações desses padrões no Visual Studio, no Azure e no Visual Studio Online:

- [Adicionar scripts ao controle de origem no Visual Studio](#vsscripts)
- [Armazenar dados confidenciais no Azure](#appsettings)
- [Usar o Git no Visual Studio e o Visual Studio Online](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Scripts de automação de tratar como código-fonte

Quando você estiver trabalhando em um projeto de nuvem que está sendo alterado coisas com frequência e você deseja ser capaz de reagir rapidamente a problemas relatados pelos clientes. Responder rapidamente envolve o uso de scripts de automação, conforme explicado no [automatizar tudo](automate-everything.md) capítulo. Todos os scripts que você usa para criar seu ambiente, implante a ele, escala, etc., precisam ser sincronizados com o código de origem do aplicativo.

Para manter os scripts em sincronia com o código, armazená-las em seu sistema de controle de origem. Em seguida, se você precisar reverter as alterações ou fazer uma correção rápida no código de produção que é diferente do código de desenvolvimento, você não precisa gastar tempo tentando rastrear quais configurações foram alteradas ou os membros da equipe tem cópias de versão que você precisa. Você tem a garantia de que os scripts que você precisa estão em sincronizado com a base de código que você precisa para e tem a garantia de que todos os membros da equipe estão trabalhando com os mesmos scripts. Em seguida, se você precisar automatizar o teste e implantação de um hot fix de produção ou desenvolvimento de um novo recurso, você terá o script certo para o código que precisa ser atualizado.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Não fazer check-in segredos

Um repositório de código fonte é geralmente acessível para muitas pessoas para que ele seja um local seguro adequadamente para os dados confidenciais, como senhas. Se os scripts contam com segredos, como senhas, parametrize essas configurações para que eles não são salvos no código-fonte e armazenam seus segredos em outro lugar.

Por exemplo, do Azure permite que fazer o download de arquivos que contêm configurações de publicação para automatizar a criação de perfis de publicação. Esses arquivos incluem nomes de usuário e senhas que estão autorizadas a gerenciar os serviços do Azure. Se você usar esse método para criar perfis de publicação, e se você verificar esses arquivos ao controle de origem, qualquer pessoa com acesso ao seu repositório pode ver os nomes de usuário e senhas. Você pode armazenar com segurança a senha no perfil de publicação em si porque ele é criptografado e está em um *. pubxml.user* arquivo que, por padrão, não está incluído no controle de origem.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Ramificações de origem de estrutura para facilitar o fluxo de trabalho de DevOps

Como implementar ramificações no seu repositório afeta sua capacidade de desenvolver novos recursos e corrigir problemas em produção. Aqui está um padrão de muita de médio porte equipes use:

![Estrutura de ramificação de origem](source-control/_static/image1.png)

A ramificação mestre sempre corresponde código na produção. Ramificações abaixo mestre correspondem a diferentes estágios do ciclo de vida de desenvolvimento. A ramificação de desenvolvimento é onde você implementa novos recursos. Para uma equipe pequena você pode ter apenas mestre e desenvolvimento, mas geralmente recomendamos que pessoas têm uma ramificação de preparo entre o desenvolvimento e mestre. Você pode usar preparo para testes de integração final antes de uma atualização é movida para a produção.

Para grandes equipes pode haver ramificações separadas para cada novo recurso; para uma equipe menor, você pode ter todos check-in para a ramificação de desenvolvimento.

Se você tiver uma ramificação para cada recurso, quando o recurso A está pronto você mesclar suas alterações de código fonte backup para o desenvolvimento branch e para baixo em outras ramificações de recurso. Esse código de origem no processo de mesclagem pode ser demorado e para evitar que funcionam mantendo recursos separados, algumas equipes implementam uma alternativa chamada  *[recurso alterna](http://en.wikipedia.org/wiki/Feature_toggle)*  (também conhecido como *recurso sinalizadores*). Isso significa que todo o código para todos os recursos é da mesma ramificação, mas você habilitar ou desabilitar cada recurso usando opções no código. Por exemplo, suponha que A do recurso é um novo campo para tarefas do aplicativo de corrigir e recurso B adiciona a funcionalidade de cache. O código para os dois recursos pode ser no branch de desenvolvimento, mas a exibição do aplicativo será somente o novo campo quando uma variável é definida como true e ele usará apenas o cache quando uma variável diferente é definida como true. Se um do recurso não está pronto para ser promovido, mas o recurso B está pronto, você pode promover todo o código de produção com a chave de recurso A desativar e alternar o recurso B em. Você pode concluir um recurso e promovê-la mais tarde, tudo com mesclagem não código fonte.

Se ou não usar ramificações ou alterna para recursos, uma estrutura de ramificação como isso permite que você fluxo seu código de desenvolvimento para a produção de uma maneira ágil e reproduzível.

Essa estrutura permite reagir rapidamente aos comentários dos clientes. Se você precisar fazer uma correção rápida para produção, você também pode fazer que eficiente de uma maneira ágil. Você pode criar uma ramificação de preparação ou mestre, e quando ele estiver pronto mesclá-lo para cima no mestre e para baixo em ramificações de desenvolvimento e de recurso.

![Ramificação de hotfix](source-control/_static/image2.png)

Sem uma estrutura de ramificação assim com sua separação de produção e ramificações de desenvolvimento, um problema de produção pode colocá-lo na posição da necessidade de promover o novo código de recurso junto com sua correção de produção. O novo código de recurso não pode ser totalmente testada e pronta para produção e talvez você precise fazer muito trabalho fazendo alterações que não estão prontas. Ou talvez você precise atrasar sua correção para testar as alterações e prepará-los implantar.

Em seguida, você verá exemplos de como implementar esses três padrões no Visual Studio, no Azure e no Visual Studio Online. Estes são exemplos, em vez de instruções passo a passo de how-a--it detalhadas; Para obter instruções detalhadas que fornecem todo o contexto necessário, consulte o [recursos](#resources) seção no final deste capítulo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Adicionar scripts ao controle de origem no Visual Studio

Você pode adicionar scripts para controle de origem no Visual Studio, incluindo-os em uma pasta de solução do Visual Studio (supondo que o projeto está no controle de origem). Aqui está uma maneira de fazer isso.

Crie uma pasta para os scripts na pasta de solução (na mesma pasta que tem o *. sln* arquivo).

![Pasta de automação](source-control/_static/image3.png)

Copie os arquivos de script na pasta.

![Conteúdo da pasta de automação](source-control/_static/image4.png)

No Visual Studio, adicione uma pasta de solução para o projeto.

![Nova seleção de menu de pasta de solução](source-control/_static/image5.png)

E adicione os arquivos de script para a pasta de solução.

![Adicionar Item existente seleção de menu](source-control/_static/image6.png)

![Adicionar item existente](source-control/_static/image7.png)

Agora, os arquivos de script são incluídos em seu projeto e controle de origem para acompanhar as alterações de versão junto com as alterações de código de origem correspondentes.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Armazenar dados confidenciais no Azure

Se você executar o aplicativo em um Site do Azure, uma maneira para evitar armazenar credenciais no controle de origem é armazená-los no Azure.

Por exemplo, o aplicativo corrigir armazena em seus Web. config arquivo duas conexão cadeias de caracteres que terão as senhas em produção e uma chave que fornece acesso à sua conta de armazenamento do Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Se você colocar os valores reais de produção para essas configurações no seu *Web. config* arquivo, ou se você pode colocá-los *Web.Release.config* arquivo para configurar uma transformação do Web. config para inseri-los durante a implantação, eles serão armazenados no repositório de origem. Se você inserir as cadeias de caracteres de conexão do banco de dados para a produção perfil de publicação, a senha estará em seu *. pubxml* arquivo. (Você pode excluir o *. pubxml* arquivo de controle de origem, mas, em seguida, você perderá o benefício de todas as outras configurações de implantação de compartilhamento.)

Azure oferece uma alternativa para o **appSettings** e seções de cadeias de caracteres de conexão a *Web. config* arquivo. Aqui está a parte relevante do **configuração** guia para um site da web no portal de gerenciamento do Azure:

![appSettings e connectionStrings no portal](source-control/_static/image8.png)

Quando você implanta um projeto para este site da web e o aplicativo é executado, quaisquer valores armazenados no Azure substituem valores que estão no arquivo Web. config.

Você pode definir esses valores no Azure usando o portal de gerenciamento ou scripts. O script de automação de criação do ambiente você viu no [automatizar tudo](automate-everything.md) capítulo cria um banco de dados do SQL Azure, obtém o armazenamento e cadeias de caracteres de conexão de banco de dados SQL e armazena esses segredos nas configurações para seu site da web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Observe que os scripts são parametrizados para que os valores reais não obter persistente para o repositório de origem.

Quando é executado localmente no seu ambiente de desenvolvimento, o aplicativo lê o arquivo Web. config local e sua conexão pontos de cadeia de caracteres para um banco de dados LocalDB SQL Server o *aplicativo\_dados* pasta do seu projeto da web. Quando você executa o aplicativo no Azure e o aplicativo tenta ler esses valores no arquivo Web. config, o que ele obtém de volta e usa são os valores armazenados para o Site da Web, não o que é, na verdade, no arquivo Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Usar o Git no Visual Studio e o Visual Studio Online

Você pode usar qualquer ambiente de controle de origem para implementar a estrutura de ramificação DevOps apresentada anteriormente. Para equipes distribuídas um [sistema de controle de versão distribuído](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) pode funcionar melhor; para outras equipes um [centralizado sistema](http://en.wikipedia.org/wiki/Revision_control) pode funcionar melhor.

[Git](http://git-scm.com/) é um DVCS que se tornou muito popular. Quando você usar Git para controle de origem, você tem uma cópia completa do repositório com todo seu histórico no computador local. Muitas pessoas preferem que porque é mais fácil continuar a trabalhar quando não estiver conectado à rede – você pode continuar a fazer é confirmada e reversões, criar e mudar as ramificações e assim por diante. Mesmo quando você estiver conectado à rede, é mais fácil e rápido criar ramificações e mudar as ramificações quando tudo o que é local. Você também pode fazer reversões e confirmações locais sem causar impacto em outros desenvolvedores. E você pode processar em lotes confirmações antes de enviá-los para o servidor.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), anteriormente conhecido como Team Foundation Service, oferece dois Git e [controle de versão do Team Foundation](https://msdn.microsoft.com/en-us/library/ms181237(v=vs.120).aspx) (TFVC; centralizado de controle de origem). Aqui na Microsoft, no grupo do Azure algumas equipes usam controle de origem centralizado, alguns use distribuído, e alguns usam uma mistura (centralizado para alguns projetos e distribuído para outros projetos). O serviço do VSO está livre para até 5 usuários. Você pode se inscrever para um plano gratuito [aqui](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 inclui interno de primeira classe [Git suporte](https://msdn.microsoft.com/en-us/library/hh850437.aspx); aqui está uma rápida demonstração de como isso funciona.

Com um projeto aberto no Visual Studio 2013, clique com botão direito a solução em **Solution Explorer**e escolha **adicionar solução ao controle de origem**.

![Adicionar solução ao controle de origem](source-control/_static/image9.png)

O Visual Studio perguntará se você deseja usar TFVC (controle de versão centralizado) ou Git.

![Escolha o controle de origem](source-control/_static/image10.png)

Quando você selecionar Git e clique em **Okey**, o Visual Studio cria um novo repositório Git local em sua pasta de solução. O novo repositório não tem arquivos ainda; Você precisa adicioná-los para o repositório, fazendo uma confirmação Git. Com o botão direito na solução **Solution Explorer**e, em seguida, clique em **confirmar**.

![Confirmação](source-control/_static/image11.png)

O Visual Studio automaticamente prepara todos os arquivos de projeto para a confirmação e listá-los no **Team Explorer** no **alterações incluídas** painel. (Se houver alguns você não deseja incluir na confirmação, você pode selecioná-los, com o botão direito e clique em **excluir**.)

![Team Explorer](source-control/_static/image12.png)

Insira um comentário de confirmação e clique em **confirmação**, e o Visual Studio executa a configuração e exibe a ID de confirmação.

![Alterações do Team Explorer](source-control/_static/image13.png)

Agora se você alterar o código para que seja diferente do que está no repositório, você pode exibir facilmente as diferenças. Atalho de um arquivo que você alterou, selecione **comparar com Unmodified**, e obter uma exibição de comparação que mostra suas alterações não confirmadas.

![Comparar com inalterado](source-control/_static/image14.png)

![Comparação mostrando alterações](source-control/_static/image15.png)

Você pode ver facilmente as alterações que você está fazendo e check-in.

Suponha que você precisa fazer uma ramificação – você poderá fazê-lo no Visual Studio. Em **Team Explorer**, clique em **nova ramificação**.

![Nova ramificação do Team Explorer](source-control/_static/image16.png)

Digite o nome de uma ramificação, clique em **criar ramificação**, e se você selecionou **ramificação de check-out**, Visual Studio verifica automaticamente o novo branch.

![Nova ramificação do Team Explorer](source-control/_static/image17.png)

Agora você pode fazer alterações nos arquivos e check-in para a ramificação. E você pode facilmente alternar entre as ramificações e o Visual Studio automaticamente os arquivos que ramificar você fez check-out de sincronizações. Neste exemplo de página da web título em  *\_cshtml* foi alterado para "Hot Fix 1" no HotFix1 ramificação.

![Ramificação Hotfix1](source-control/_static/image18.png)

Se você alternar de volta para o mestre de filiais, o conteúdo do  *\_cshtml* arquivo revertida automaticamente para o que são a ramificação mestre.

![Ramificação mestre](source-control/_static/image19.png)

Este um exemplo simples de como você pode criar rapidamente uma ramificação e alternar entre ramificações. Esse recurso permite que um fluxo de trabalho altamente agile usando a estrutura de ramificação e scripts de automação apresentadas a [automatizar tudo](automate-everything.md) capítulo. Por exemplo, você pode estar trabalhando no branch de desenvolvimento, criar uma ramificação de hotfix do mestre, alterne para o novo branch, fazer as alterações e confirmá-las e alterne de volta para a ramificação de desenvolvimento e continuar o que estava fazendo.

O que é visto aqui é como você trabalha com um repositório Git local no Visual Studio. Em um ambiente de equipe você normalmente também enviar alterações por push um repositório comum. As ferramentas do Visual Studio também permitem que você apontar para um repositório Git remoto. Você pode usar GitHub.com para essa finalidade, ou você pode usar [Git no Visual Studio Online](https://msdn.microsoft.com/en-us/library/hh850437.aspx) integrado com todos os outros recursos Online do Visual Studio, como o item de trabalho e de monitoramento de erros.

Não é a única maneira que você pode implementar uma estratégia de ramificação agile, claro. Você pode habilitar o mesmo fluxo de trabalho agile usando um repositório de controle de origem centralizado.

## <a name="summary"></a>Resumo

Medir o sucesso do seu sistema de controle de origem com base em como rapidamente, você pode fazer uma alteração e obtê-lo ao vivo de uma maneira segura e previsível. Se você estiver assustados fazer uma alteração, porque você deve fazer um ou dois dias de teste manual nele, você pode se perguntar o que você precisa fazer process-wise ou test-wise para que você pode fazer essa alteração em minutos ou em pior não mais de uma hora. Uma estratégia para fazer isso é implementar a integração contínua e fornecimento contínuo, o que abordaremos no [próximo capítulo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Recursos

O [Visual Studio Online](https://www.visualstudio.com/) portal fornece serviços de suporte e documentação, e você pode se inscrever para uma conta. Se você tiver o Visual Studio 2012 e gostaria de usar o Git, consulte [Visual Studio Tools para Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Para obter mais informações sobre TFVC (controle de versão centralizado) e o Git (controle de versão distribuídos), consulte os seguintes recursos:

- [Qual sistema de controle de versão devo usar: TFVC ou Git?](https://msdn.microsoft.com/en-us/library/vstudio/ms181368.aspx#tfvc_or_git_summary) Documentação do MSDN, inclui uma tabela que resume as diferenças entre TFVC e Git.
- [Bem, como o Team Foundation Server e como o Git, mas que é melhor?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Comparação de TFVC e Git.

Para obter mais informações sobre estratégias de expansão, consulte os seguintes recursos:

- [Criando um Pipeline de versão com o Team Foundation Server 2012](https://msdn.microsoft.com/en-us/library/dn449957.aspx). Documentação do Microsoft Patterns e práticas recomendadas. Consulte o capítulo 6 para obter uma discussão sobre estratégias de expansão. O recurso defensores alterna sobre ramificações de recurso e se ramificações para recursos forem usadas, advogados mantê-los curta duração (em horas ou dias, no máximo).
- [Guia de controle de versão](https://aka.ms/vsarsolutions). Guia para estratégias de expansão por ALM Rangers. Consulte Strategies.pdf ramificação na guia Downloads.
- [Desenvolvimento de software com o recurso alterna](https://msdn.microsoft.com/en-us/magazine/dn683796.aspx). Artigo da MSDN Magazine.
- [Alternância de recurso](http://martinfowler.com/bliki/FeatureToggle.html). Introdução ao recurso alterna / recurso sinalizadores no blog de Martin Fowler.
- [Recurso vs alternâncias ramificações de recurso](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Outro postagem de blog sobre alternâncias de recurso, por Dylan Smith.

Para obter mais informações sobre como lidar com informações confidenciais que não devem ser mantidas em repositórios de controle de origem, consulte os seguintes recursos:

- [Práticas recomendadas para a implantação de senhas e outros dados confidenciais em ASP.NET e o serviço de aplicativo do Azure](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Sites do Azure: Como cadeias de caracteres de aplicativo e Conexão cadeias de caracteres trabalho](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explica o recurso do Azure que substitui `appSettings` e `connectionStrings` dados o *Web. config* arquivo.
- [Personalizar as configurações de aplicativo e nos Sites do Azure - com Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Para obter informações sobre outros métodos de informações confidenciais de manutenção do controle de origem, consulte [ASP.NET MVC: manter privada configurações de controle de origem](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

>[!div class="step-by-step"]
[Anterior](automate-everything.md)
[Próximo](continuous-integration-and-continuous-delivery.md)
