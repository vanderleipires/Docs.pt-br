---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Criação de um Farm de servidores com o Framework do Web Farm | Microsoft Docs
author: jrjlee
description: Este tópico descreve como usar o Web Farm Framework (WFF) 2.0 para criar e configurar um farm de servidores web de uma coleção de servidores.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: ad557306cb4d3c62932c640b598f82d692bc74cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388287"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Criação de um Farm de servidores com o Web Farm Framework
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como usar o Web Farm Framework (WFF) 2.0 para criar e configurar um farm de servidores web de uma coleção de servidores.


WFF permite sincronizar os produtos de plataforma da web e componentes, aplicativos web, sites e as definições de configuração em vários servidores web com balanceamento de carga. Em cenários em que você precisa de mais de um servidor web, como ambientes de preparo e produção, isso pode simplificar muito o processo de implantação e configuração. Você pode implantar um aplicativo web em um único servidor&#x2014;o *servidor primário*&#x2014;e WFF automaticamente serão replicadas para esse aplicativo web em todos os outros servidores de web no farm de servidores.

## <a name="understanding-the-web-farm-framework"></a>Noções básicas sobre a estrutura de Farm da Web

Você pode usar WFF 2.0 para provisionar, gerenciar e implantar conteúdo em um grupo de servidores web. Uma implantação de WFF consiste em três principais funções do servidor:

- O *servidor do controlador*. Você pode usar esse servidor para criar e configurar os farms de servidores WFF. O servidor de controlador gerencia a sincronização de componentes de plataforma da web, as definições de configuração e de aplicativos entre os servidores web em um farm de servidores. Instalar WFF 2.0 no servidor de controlador, e o servidor do controlador serão, por sua vez, instale o agente WFF em cada um dos servidores em um farm de servidores. O servidor do controlador não conceitualmente pertence a um farm de servidores WFF e um servidor de controlador único pode gerenciar vários farms de servidores. Nesse cenário, você pode usar um único servidor de controlador WFF para criar e gerenciar o farm de servidores de preparo e o farm de servidores de produção.
- O *servidor primário*. Cada farm de servidores WFF inclui um único servidor primário. Quando você instala componentes de plataforma da web ou implantar aplicativos para o servidor primário, o WFF sincroniza as alterações para todos os outros servidores no farm de servidores.
- O *servidor secundário*. Cada farm de servidores WFF inclui um ou mais servidores secundários. As alterações feitas ao servidor primário são replicadas para todos os servidores do farm de servidores secundários.

Isso mostra como essas funções de servidor se relacionam com os ambientes de preparo e produção Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

Nesse cenário, o ambiente de preparo e o ambiente de produção são configurados como farms de servidores WFF. Um único servidor de controlador WFF gerencia os dois farms. Em cada farm de servidores, todas as alterações para o servidor primário são replicadas para todos os servidores secundários.

Antes de começar a configurar seus ambientes de preparo e produção, recomendamos que você leia estes artigos para se familiarizar com os principais conceitos de WFF 2.0:

- [Visão geral do Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Como configurar um Farm de servidores com o Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisitos de sistema e plataforma para o Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Visão geral da tarefa

Para concluir as tarefas e instruções passo a passo neste tópico, você precisará de pelo menos três servidores&#x2014;um controlador WFF, um servidor web principal para o farm de servidores e um ou mais servidores web secundário para o farm de servidores. Você pode adicionar mais servidores secundários a um farm de servidores WFF a qualquer momento. Em um alto nível, para criar e configurar um farm de servidores WFF para o seu ambiente de preparo ou produção, que você precisará:

- Crie um servidor do controlador por meio da instalação dos serviços de informações da Internet (IIS) 7.5 e WFF 2.0.
- Prepare os servidores primário e secundário, criando uma conta de administrador comuns e configurando exceções de firewall.
- Configure o farm de servidores usando o Gerenciador do IIS no servidor de controlador.
- Configure o balanceamento de carga usando o IIS roteamento ARR (Application Request) ou uma tecnologia alternativo de balanceamento de carga.

As tarefas e instruções passo a passo neste tópico pressupõem que você está começando com compilações de servidor limpo executando o Windows Server 2008 R2. Antes de começar, para cada servidor, certifique-se de que:

- Windows Server 2008 R2 Service Pack 1 e todas as atualizações disponíveis serão instaladas.
- O servidor está ingressado no domínio.
- O servidor tem um endereço IP estático.

> [!NOTE]
> Para obter mais informações sobre como adicionar computadores a um domínio, consulte [ingressando computadores no domínio e fazendo logon](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obter mais informações sobre como configurar endereços IP estáticos, consulte [configurar um endereço IP estático](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Criar o servidor de controlador WFF

Para criar um servidor de controlador WFF, você precisará instalar o IIS 7 ou posterior e WFF 2.0 ou posterior. Nos bastidores, WFF usa a ferramenta de implantação da Web de IIS (implantação da Web) 2. x para sincronizar os servidores no farm. Se você usar o Web Platform Installer para instalar o WFF, o instalador será baixar e instalar a implantação da Web para você automaticamente.

**Para criar o servidor de controlador WFF**

1. Baixe e instale o [Web Platform Installer](https://go.microsoft.com/?linkid=9739157).
2. Na parte superior do **Web Platform Installer 3.0** janela, clique em **produtos**.
3. No lado esquerdo da janela, no painel de navegação, clique em **Server**.
4. No **configuração recomendada do IIS 7** de linhas, clique em **Add**.
5. No <strong>Web Farm Framework 2.</strong> <em>x</em> de linhas, clique em <strong>Add</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Clique em **Instalar**. Observe que o Web Platform Installer adicionou a ferramenta de implantação da Web, juntamente com várias outras dependências para a lista de instalação.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Examine os termos de licença e se você concordar com os termos, clique em **aceito**.
8. Quando a instalação for concluída, clique em **terminar**e, em seguida, feche o **Web Platform Installer 3.0** janela.

## <a name="configure-the-primary-and-secondary-servers"></a>Configurar os servidores primários e secundários

Antes de criar um farm de servidores WFF, você deve concluir algumas tarefas de preparação nos servidores web que farão parte do farm:

- Adicionar exceções de firewall para permitir que o **Core Networking**, **administração remota**, e **compartilhamento de arquivo e impressora** recursos para se comunicar com o servidor do controlador WFF .
- Criar uma conta de domínio (por exemplo, **FABRIKAM\stagingfarm**) no Active Directory e adicione-o ao grupo de administradores locais em cada servidor. Você usará essa conta como a conta de administrador de farm de servidor ao criar o farm de servidores.

Para obter mais informações sobre como configurar essas exceções de firewall no Firewall do Windows, consulte [requisitos de sistema e plataforma para o Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805128). Para outros sistemas de firewall, consulte a documentação do produto.

Você pode usar o procedimento a seguir para adicionar uma conta de domínio ao grupo Administradores local no Windows Server 2008 R2. Você deve executar esse procedimento em cada servidor que você deseja adicionar ao farm de servidores&#x2014;em outras palavras, adicione a mesma conta de domínio para o grupo de administradores locais no servidor primário e em cada servidor secundário.

**Para adicionar uma conta de domínio ao grupo de administradores locais**

1. Sobre o **inicie** , aponte para **ferramentas administrativas**e, em seguida, clique em **Gerenciador do servidor**.
2. No **Gerenciador de servidores** janela, no painel de exibição de árvore, expanda **Configuration**, expanda **usuários e grupos locais**e, em seguida, clique em **grupos**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. No **grupos** painel, clique duas vezes em **administradores**.
4. No **propriedades de administradores** caixa de diálogo, clique em **Add**.
5. No **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, digite (ou navegue) à sua conta de domínio (por exemplo, **FABRIKAM\stagingfarm**) e, em seguida, clique em **Okey**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. No **propriedades de administradores** caixa de diálogo, clique em **Okey**.

Os servidores agora estão prontos para ser adicionado a um farm de servidores. No caso do servidor primário, você pode configurar o servidor para atender aos requisitos de aplicativo, antes ou depois de criar o farm de servidores&#x2014;em ambos os casos, o WFF sincronizará os servidores ao implantar o mesmo produtos, componentes ou configuração para os servidores secundários. Para simplificar, este tutorial presume que você vai configurar o servidor primário quando você terminar de criar o farm de servidores.

## <a name="create-the-wff-server-farm"></a>Criar o Farm de servidores WFF

Neste ponto, todos os seus servidores estão prontos para ser adicionado a um farm de servidores WFF:

- Você instalou WFF no servidor de controlador.
- Você configurou as exceções de firewall em seus servidores web primários e secundários.
- Você adicionou uma conta de domínio ao grupo de administradores locais em seus servidores web primários e secundários.

A próxima etapa é criar o farm de servidores em WFF. Você pode fazer isso do Gerenciador do IIS no servidor de controlador WFF.

**Para criar um farm de servidores WFF**

1. No servidor de controlador WFF, sobre o **iniciar** , aponte para **ferramentas administrativas**e, em seguida, clique em **serviços de informações da Internet (IIS) Manager**.
2. No **conexões** painel, expanda o nó de servidor local, clique com botão direito **Farms de servidores**e, em seguida, clique em **criar Farm de servidores**.
3. No **criar Farm de servidores** diálogo caixa, digite um nome significativo para o farm de servidores (por exemplo, **Farm de preparo**) e, em seguida, selecione **provisionar farm de servidores**.
4. Digite o nome de usuário e a senha da conta de domínio que você adicionou ao grupo Administradores local em cada servidor.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Clique em **Avançar**.
6. Sobre o **adicionar servidores** página, digite o nome de domínio totalmente qualificado (FQDN) do servidor primário, selecione **servidor primário**e, em seguida, clique em **adicionar**.
7. Neste ponto, WFF tentará contatar o servidor primário, usando as credenciais fornecidas. Se a conexão for bem-sucedida, o servidor primário será adicionado à tabela sobre o **adicionar servidores** página.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Você deve ter notado que **Server está disponível para balanceamento de carga** é selecionada por padrão. WFF usa o módulo de ARR do IIS para implementar o balanceamento de carga e, assim, distribuir solicitações entre servidores web no seu farm de servidores. Na maioria dos cenários, apenas desmarque os **Server está disponível para balanceamento de carga** opção se você quiser usar uma em vez disso, solução de balanceamento de carga de terceiros.
8. Sobre o **adicionar servidores** página, digite o FQDN do servidor secundário primeiro e, em seguida, clique em **Add**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Repita a etapa 7 para todos os servidores secundários adicionais em seu farm e, em seguida, clique em **concluir**.

Seu farm de servidores WFF está agora em execução. Todos os produtos de plataforma da web ou componentes que podem ser instalados no servidor primário e quaisquer aplicativos da web ou conteúdo que você implanta para o servidor primário, serão provisionados automaticamente em todos os servidores secundários.

WFF é um tópico amplo e complexo, e você pode aprender mais sobre ele na [Microsoft Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805129) site. Por enquanto, no entanto, há duas áreas de recursos que você precisa estar atento:

- *Provisionamento de aplicativo* é o processo que replica o conteúdo do servidor primário, como aplicativos web e definições de configuração, em todos os servidores secundários no farm de servidores. Por exemplo, se você implantar a solução de exemplo do Contact Manager no servidor de preparo primário, o processo de provisionamento de aplicativo WFF implantará essa solução para todos os servidores de preparo secundários. Por padrão, o processo de provisionamento de aplicativo é executado a cada 30 segundos.
- *Provisionamento de plataforma* é o processo que sincroniza os produtos de plataforma da web e componentes do servidor primário para todos os servidores secundários no farm de servidores. Por exemplo, se você instalar o ASP.NET MVC 3 em seu servidor de preparo primário, o processo de provisionamento de plataforma usará o Web Platform Installer para instalar o ASP.NET MVC 3 em todos os servidores de preparo secundários. Por padrão, o processo de provisionamento de plataforma é executado a cada cinco minutos.

Você pode gerenciar configurações de provisionamento de plataforma e de aplicativo básico do Gerenciador do IIS em seu servidor do controlador WFF.

**Explorar o aplicativo e configurações de provisionamento de plataforma**

1. No Gerenciador do IIS, nos **conexões** painel, selecione seu farm de servidores.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. No **Farm de servidores** painel, clique duas vezes em **provisionamento de aplicativo**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Como você pode ver, o farm de servidores está atualmente configurado para sincronizar as configurações da web de conteúdo e configuração entre o servidor primário e os servidores secundários a cada 30 segundos.
4. Clique em **volta**e, em seguida, clique duas vezes em **provisionamento de plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Como você pode ver, o farm de servidores está atualmente configurado para sincronizar os produtos de plataforma da web e componentes entre o servidor primário e os servidores secundários a cada cinco minutos.
6. Clique em **volta**.
7. Para forçar o farm de servidores para sincronizar os produtos de plataforma da web imediatamente, na **ações** painel, clique em **provisionar plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Provisionamento de plataforma pode levar algum tempo. O processo do instalador é executado em segundo plano nos servidores secundários no seu farm de servidores.
8. Depois que você autorizar tempo suficiente para concluir o processo de provisionamento, você pode verificar se os produtos e componentes que você adicionou ao servidor primário agora foram replicadas nos servidores secundários. Por exemplo, você pode fazer logon em um servidor secundário e usar o **Gerenciador do servidor** para verificar se a função servidor web foi instalada.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Você também pode verificar a lista de programas instalados para verificar se foram adicionados diversos componentes de plataforma da web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurar balanceamento de carga

Quando você cria um web farm, você precisará configurar alguma forma de balanceamento de carga para distribuir solicitações HTTP entre os servidores web. Isso pode ser o Windows Server 2008 de balanceamento de carga, ARR do IIS, ou produtos de terceiros com base em hardware ou software de balanceamento de carga de solução.

WFF foi projetado para integração estreita com o IIS ARR. Para aproveitar essa integração, você precisa instalar o módulo do ARR no servidor de controlador WFF. Você direcionar todo o tráfego da web para o servidor de controlador, normalmente ao configurar registros de sistema de nome de domínio (DNS). O servidor do controlador, em seguida, distribui as solicitações de entrada entre os servidores em seu farm, com base na disponibilidade do servidor e vários outros critérios.

> [!NOTE]
> Você não precisa usar o ARR com WFF; Você pode configurar WFF para trabalhar com soluções de balanceamento de carga de terceiros. Para obter mais informações, consulte [visão geral do Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805126).


Balanceamento de carga usando ARR é um tópico complexo, mais do que está além do escopo deste tutorial. No entanto, você pode usar o procedimento a seguir para instalar o módulo do ARR e começar a trabalhar com balanceamento de carga.

**Para configurar o balanceamento de carga no servidor de controlador WFF**

1. No servidor de controlador WFF, inicie o Web Platform Installer.
2. Na parte superior do **Web Platform Installer 3.0** janela, clique em **produtos**.
3. No lado esquerdo da janela, no painel de navegação, clique em **Server**.
4. No **aplicativo solicitar roteamento 2.5** de linhas, clique em **Add**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Clique em **instale**e, em seguida, siga as instruções na **Web Platform Installation** janela.
6. Quando a instalação for concluída, inicie o Gerenciador do IIS e, na **conexões** painel, clique em seu nó do farm de servidor. Observe que foram adicionados diversos novos ícones para o **Farm de servidores** painel.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. No **Farm de servidores** painel, clique duas vezes em **balanceamento de carga**.
8. No **balanceamento de carga** algoritmo equilibrar o painel, selecione uma carga (por exemplo, **solicitação atual menos**).

    > [!NOTE]
    > Para obter mais informações sobre algoritmos e outras definições de configuração de balanceamento de carga, consulte [módulo de roteamento de solicitação de aplicativo](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. No **ações** painel, clique em **aplicar**.

Agora você configurou para os servidores no seu farm de servidores de balanceamento de carga. Se você direcionar todo seu web farm o tráfego para o servidor do controlador, as solicitações serão distribuídas entre os servidores no seu farm de acordo com a disponibilidade e o algoritmo de balanceamento de carga que você selecionou.

Para obter mais informações sobre como configurar o balanceamento de carga com ARR, consulte [módulo de roteamento de solicitação de aplicativo](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorar o Farm de servidores

Você pode monitorar a integridade do seu farm de servidores a qualquer momento por meio do Gerenciador do IIS no servidor de controlador. No **conexões** painel, expanda seu farm de servidores e, em seguida, clique em **servidores**. O painel central mostrará um resumo de cada servidor no farm junto com um log de rastreamento da atividade recente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusão

Seu farm de servidores WFF agora deve estar em execução. Você pode configurar o servidor primário para dar suporte à abordagem de implantação que você prefira&#x2014;consulte a seção leitura adicional para obter detalhes&#x2014;e sua configuração será replicada em cada servidor secundário no farm de servidores.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre todos os aspectos de configuração e uso o WFF, consulte o [Microsoft Web Farm Framework 2.0 para o IIS 7](https://go.microsoft.com/?linkid=9805129) site.

> [!div class="step-by-step"]
> [Anterior](configuring-a-database-server-for-web-deploy-publishing.md)
> [Próximo](configuring-deployment-properties-for-a-target-environment.md)
