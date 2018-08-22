---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Melhorias no Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de aprimoramentos e melhorias para projetos da Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 60259ceb99de536410aa5f53db64fb2dca68bf66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834869"
---
<a name="improvements-in-visual-studio-2005"></a>Melhorias no Visual Studio 2005
====================
por [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de aprimoramentos e melhorias para projetos da Web.


Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de aprimoramentos e melhorias para projetos da Web. Tão potente assim como o Visual Studio .NET 2002 e 2003, havia muitas reclamações da maneira que os projetos da Web foram tratados. Visual Studio 2005 adiciona um número significativo de novos recursos para solucionar esses reclamações. Para aqueles que preferem a maneira que o Visual Studio .NET 2003 manipulados compilação de aplicativos da Web, consulte [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).

Neste módulo, abordaremos melhorias na criação do projeto da Web, gerenciamento e desenvolvimento. Em um módulo posterior, também abrangem melhorias na criação de projetos da Web e implantá-los.

## <a name="frontpage-server-extensions"></a>Extensões de servidor do FrontPage

Visual Studio .NET 2002 e 2003 necessários para extensões FrontPage Server Extensions na caixa para criar ou compilar projetos da Web. Os desenvolvedores tinham uma escolha entre dois modos de acesso diferentes (modo de acesso de arquivo ou o FrontPage Server Extensions), ambos usados extensões FrontPage Server Extensions para executar tarefas como definir a raiz do aplicativo no IIS, etc.

Visual Studio 2005 remove a dependência de extensões FrontPage Server Extensions para projetos locais. Agora, o Visual Studio 2005 acessa a metabase do IIS diretamente, em vez de usar as extensões de servidor do FrontPage. Visual Studio 2005 também adiciona suporte para FTP que permite acesso de projeto remoto sem a necessidade de extensões FrontPage Server Extensions.

Para os desenvolvedores que desejam usar extensões FrontPage Server Extensions em seus projetos, a opção é ainda estão disponível. No entanto, com base nos comentários forte da comunidade de desenvolvedores do ASP.NET, não é um requisito.

> [!NOTE]
> Extensões FrontPage Server Extensions ainda são necessárias para a criação de projeto remoto, abertura, etc.


## <a name="aspnet-development-server"></a>ASP.NET Development Server

Visual Studio 2005 vem com um novo servidor de Web chamado ASP.NET Development Server. (Esse servidor Web era conhecido anteriormente como Cassini).

Há vários benefícios de se o ASP.NET Development Server.

- Agora é possível para administradores desenvolver e depurar em um servidor Web.
- O ASP.NET Development Server mapeia dinamicamente os diretórios virtuais em qualquer local no sistema de arquivos, permitindo a locais de projeto flexível.
- Os usuários no Windows XP Professional que já estiver usando o IIS agora será capazes de criar novos aplicativos Web que não afetam a estrutura de arquivo ou pasta do seu Site Web padrão no IIS.

Nenhuma configuração especial é necessária para aproveitar o ASP.NET Development Server. Quando um projeto Web que está hospedado no sistema de arquivos é depurado ou procurado, o Visual Studio 2005 iniciará automaticamente uma instância do servidor de desenvolvimento ASP.NET em uma porta aleatória para atender à solicitação.

Obter mais informações serão abordadas no ASP.NET Development Server neste módulo.

## <a name="improved-file-management"></a>Gerenciamento de arquivos aprimorado

No Visual Studio 2002 e 2003, um arquivo de projeto (. vbproj para o VB.NET) e. csproj para c# informações armazenadas em todos os arquivos no aplicativo Web. A exibição do Gerenciador de soluções baseia-se as informações do arquivo no arquivo de projeto. Por isso, o Gerenciador de soluções geralmente exibiria informações imprecisas em casos em que os editores externos foram usados. O Visual Studio 2002 e 2003 seria geralmente substituir alterações de arquivo ou não exibir a versão mais recente dos arquivos.

Visual Studio 2005 faz imediatamente com o arquivo de projeto. Em vez disso, ele lê as informações de arquivo e pasta diretamente do disco, resultando em uma exibição precisa dos arquivos em seu projeto. Porque a pasta de referências no Visual Studio 2002 e 2003 não representa uma pasta real em seu aplicativo Web, o Visual Studio 2005 também remove a pasta referências no Gerenciador de soluções. Para acessar as referências para o seu projeto no Visual Studio 2005, você deve usar as páginas de propriedades para o projeto.

## <a name="creating-web-projects"></a>Criar projetos Web

Os desenvolvedores da Web têm muitas novas opções disponíveis para a criação do projeto no Visual Studio 2005. Sites da Web agora podem ser criados em qualquer lugar no sistema de arquivos e, em seguida, podem ser depurados ou navegados usando o novo servidor de desenvolvimento do ASP.NET. Os desenvolvedores também podem criar novos sites da Web usando FTP.

Clique aqui para exibir uma vídeo passo a passo da criação de projetos da Web no Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Projetos do sistema de arquivos

Como você viu no passo a passo de vídeo, você pode optar por criar sites da Web no sistema de arquivos no computador local ou em um local remoto por meio de um compartilhamento de arquivos. Sites da Web que são criados no sistema de arquivos são acessados e depurados com o ASP.NET Development Server.

> [!NOTE]
> O ASP.NET Development Server pode causar alguma confusão para os clientes. Se um projeto Web é criado no sistema de arquivos na estrutura de diretório IISs (ou seja, c: / inetpub/wwwroot), o site da Web ainda será possível procurar por meio do ASP.NET Development Server quando iniciado a partir do Visual Studio 2005. Portanto, qualquer configuração do IIS (ou seja, métodos de autenticação) não é aplicável.


O projeto da web padrão também remove muito a sobrecarga por inclui apenas uma página Default. aspx, o arquivo default.cs e uma pasta de Data. O Web. config e pastas especiais (ou seja, Code) são adicionadas como eles são necessários. Seu projeto da web inclui apenas os arquivos e pastas que você precisa.

### <a name="http-projects"></a>Projetos HTTP

Projetos HTTP podem ser projetos que são criados em um site do IIS local ou em um site remoto. É o local do projeto padrão `http://localhost`. Se você clicar no botão Procurar, há duas opções de HTTP: IIS Local e o Site remoto. A principal diferença nessas duas opções é o método no qual as informações do site da web são exibidas na caixa de diálogo Escolher local e em como os arquivos são copiados para o servidor Web.

A opção de Local IIS lê as informações do site da metabase no computador local e os arquivos são copiados usando o sistema de arquivos. A opção de Site remoto usa as extensões de servidor do FrontPage e as informações do site e os arquivos são copiados usando HTTP e chamadas de RPC de extensões de servidor do FrontPage.

> [!NOTE]
> O arquivo vs###/_tmp.htm e get/_aspx/_ver.aspx não são usados para determinar as informações de versão.


A opção de HTTP padrão é o Local do IIS. Essa opção lê a Metabase do IIS para determinar quais sites estão disponíveis e o local no qual criar o conteúdo. Você pode selecionar uma pasta diferente ou um diretório virtual, selecionando-o na exibição de árvore. Você pode também criar um novo diretório virtual, marque pastas como aplicativos, bem como excluir diretórios virtuais existentes nessa caixa de diálogo.


![A escolha da caixa de diálogo local](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: A escolha da caixa de diálogo local


Ao contrário em versões anteriores do Visual Studio, se você verificar a **usar Secure Sockets Layer** caixa de seleção e o certificado SSL não coincide com a URL que você está pesquisando, você verá uma caixa de diálogo alerta de segurança, perguntando se você faria gostaria de continuar. Usando o Visual Studio .NET 2003, se o certificado não era uma correspondência, criando o projeto falharia.


![Alerta certificado de SSL sobre segurança](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: alerta de segurança sobre o certificado SSL


### <a name="note-on-host-headers"></a>Observação sobre cabeçalhos de Host

Se você estiver criando um aplicativo Web em um site associado a um IP específico, você precisará garantir que um cabeçalho de host esteja configurado. Caso contrário, o Visual Studio criará o site em `http://localhost`, mas o endereço IP não serão resolvidas corretamente quando o site é pesquisado ou depurado de dentro do IDE.

Se você selecionar a opção de Site remoto, a caixa de diálogo é alterado para permitir que você insira a URL de destino para o novo site da Web. Essa URL deve estar em um servidor que tenha habilitado o FrontPage Server Extensions. Se você quiser trabalhar com seu servidor Web local usando o FrontPage Server Extensions, você pode usar a opção de Site remoto e especificar uma URL local.


![Criando um Site da Web em um servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: Criando um Site da Web em um servidor remoto


Ao criar um aplicativo em um site remoto via SSL, se o certificado SSL não corresponder, a caixa de diálogo de confirmação é ligeiramente diferente do que a caixa de diálogo exibida ao usar a opção de IIS Local.


![O alerta de segurança do Site remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: O alerta de segurança do Site remoto


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 apresenta a opção de criar sites da Web via FTP. Quando você usa essa opção, o IDE cria os arquivos localmente na pasta temp de usuários e, em seguida, usa o FTP para mover os arquivos para o local de FTP.

> [!NOTE]
> O local da pasta temp é c: / Documents and Settings /&lt;usuário&gt;/Local Temp/configurações/VWDWebCache/&lt;Server&gt;/_&lt;nome do aplicativo&gt;


Ao usar a opção de FTP, você verá uma caixa de diálogo Escolher local. Você pode inserir as informações de conexão de FTP necessárias essa caixa de diálogo, conforme mostrado abaixo.


![A escolha da caixa de diálogo de local de FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: A escolha da caixa de diálogo de local de FTP


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratório: Configuração FTP do site e crie um projeto

As seguintes etapas configuram o site FTP para que um usuário tenha um local que só eles podem carregar por meio de FTP.

### <a name="install-the-ftp-service"></a>Instalar o serviço FTP

1. Abra Adicionar ou remover programas, selecione Adicionar/remover componentes do Windows
2. Selecione os serviços de informações da Internet (servidor de aplicativos no Windows 2003) e clique em **detalhes**.
3. Verifique **serviço de protocolo FTP (File Transfer)** e clique em **Okey**.
4. Clique em **próxima** para instalar o serviço FTP.

### <a name="create-a-new-folder-for-content"></a>Crie uma nova pasta para o conteúdo

1. No Windows Explorer, crie uma nova pasta denominada **User1** dentro de c: / inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configure pastas e permissões nas pastas.

1. Abra o snap-in de serviços de informações da Internet em Ferramentas administrativas. Agora, você terá uma pasta de Sites FTP sob o nó de nome do computador.
2. Expandir **Sites FTP**.
3. Clique com botão direito a **Site FTP padrão**, selecione **New**, em seguida, **Diretório Virtual**, em seguida, clique em **próxima**.
4. Insira **User1** para o nome do diretório virtual e clique **próxima**.
5. Insira **c: / inetpub/wwwroot/User1** para o caminho e clique **próxima**.
6. Clique em **próxima** e, em seguida **concluir** para concluir o assistente.
7. Clique com botão direito do **User1** diretório virtual sob o Site FTP padrão e selecione **propriedades**.
8. Verifique as **escrever** caixa de seleção e clique em **Okey** para fechar a caixa de diálogo.
9. Clique com botão direito **Site FTP padrão** e selecione **propriedades**.
10. Sobre o **contas de segurança** guia, desmarque a opção **permitir conexões anônimas**.
11. Clique em **Sim** na caixa de diálogo perguntando se deseja continuar.
12. Clique em **Okey** para fechar a caixa de diálogo.
13. Expanda o **Site padrão** sob o **Sites da Web** nó.
14. Clique com botão direito do **User1** diretório e selecione **propriedades**
15. No **configurações do aplicativo** seção, clique em **criar** para marcar a pasta do aplicativo.
16. Clique em **Okey** para fechar a caixa de diálogo.
17. Feche o snap-in de serviços de informações da Internet.

### <a name="create-web-project"></a>Criar o projeto da web

1. Abra o Visual Studio 2005.
2. Dos **arquivo** menu, selecione **New Web Site**.
3. No **local** lista suspensa, selecione **FTP**.
4. Clique em **Procurar**.
5. Insira **localhost** na **Server** caixa de texto.
6. Insira **User1** na caixa de texto de diretório.
7. Clique em **aberto**. O local do FTP será inserido na caixa de diálogo Novo Site da Web.
8. Clique em **OK**.
9. Desmarque a opção **Anonymous Logon** na caixa de diálogo logon no FTP, insira suas credenciais e clique em **Okey**.
10. O que é a URL para o projeto? (A URL para o projeto será exibido no Gerenciador de soluções.)
11. Dos **construir** menu, selecione **Build Web Site** ou **compilar solução**.
12. Clique duas vezes em Default. aspx no Gerenciador de soluções e selecione **exibir no navegador**.
13. Na caixa de diálogo de URL do Site da Web é necessária, insira `http://localhost/user1` para a URL e clique em **Okey**.

> [!NOTE]
> Se você receber um erro que indica a impossibilidade de carregar o tipo /_Default, certifique-se de que você está executando o ASP.NET 2.0 em seu site da Web e não uma versão anterior. Você pode fazer isso a partir da guia do ASP.NET nos serviços de informações da Internet.


## <a name="opening-web-projects"></a>Abrindo projetos da Web

Abrir projetos Web é semelhante à criação de projetos. As seções a seguir destacar as áreas para manter o olho para enquanto estiver trabalhando dentro do IDE. Ele também aborda o trabalho com projetos da Web usando HTTP e FTP.

Para abrir um projeto da Web, selecione Abrir Site da Web no menu arquivo. Você será solicitado com a mesma caixa de diálogo Escolher local abordada anteriormente e você tem as mesmas quatro opções disponíveis para você: Site remoto, Local IIS, FTP e sistema de arquivos.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de arquivos

Como indicado anteriormente neste módulo, o Visual Studio não usa um arquivo de projeto. Portanto, se você optar por abrir um site do sistema de arquivos, você realmente tem a opção de escolher qualquer pasta que você deseja, mesmo se a pasta que você escolher não foi criada como um projeto Web inicialmente no Visual Studio. Por exemplo, você pode optar por abrir a pasta Meus documentos como um site da Web e o Visual Studio Felizmente abri-lo e exibir os arquivos, conforme mostrado abaixo.


![Meus documentos abertos como um Site da Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Meus documentos* aberto como um Site da Web


Porque o Visual Studio cria somente outros arquivos e pastas quando necessário, sem arquivos ou pastas adicionais são adicionadas para o local em que você abrir. Um efeito colateral dessa arquitetura é que ele impede a sites da Web de aninhamento no sistema de arquivos. Por exemplo, considere a seguinte estrutura de diretório.

Projeto da Web em c: / MyWebSite

Outro projeto da web em c: / MyWebSite/aninhado

Quando você abre o site em c: / MyWebSite, a pasta Nested aparecerá como uma subpasta do aplicativo.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Ao abrir sites da Web via HTTP, as configurações são lidas a partir da metabase do IIS (IIS Local) ou usando o FrontPage Server Extensions (Site remoto). Se houver aplicativos web aninhadas, eles são exibidos, bem com um ícone que as identifica como um aplicativo. Se você estiver familiarizado com a trabalhar com aplicativos web do FrontPage, o comportamento no Visual Studio 2005 é semelhante.

Mesmo que o Visual Studio exibirá um ícone para aplicativos que estão aninhadas sob o aplicativo que está aberto no momento dentro do IDE, ele não permitirá que você expanda-las para ver seu conteúdo. No entanto, você pode clicar duas vezes neles para abri-los. Quando você fizer isso, você verá uma caixa de diálogo solicitando que você abrir o aplicativo web (e substituir a solução atualmente aberta) ou adiciona o aplicativo Web à sua solução atual.


![Clicar duas vezes em um ícone do aplicativo aninhada apresenta esta caixa de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: clicar duas vezes em um ícone do aplicativo aninhada apresenta esta caixa de diálogo


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Site FTP

Quando você abre um site por meio de FTP, os arquivos são todos copiados localmente em sua pasta temporária. O caminho completo para o local de armazenamento local é exibido no painel de propriedades do projeto e é criado usando o seguinte formato.

C: / Documents and Settings /&lt;usuário&gt;/Local Temp/configurações/VWDWebCache/&lt;Server&gt;/_&lt;nome do aplicativo&gt;

Ao usar o FTP, o Visual Studio será preciso especificar a URL base para o seu projeto para que você pode procurar conforme mostrado abaixo. Se você não especificar uma URL base, o Visual Studio perguntará para ele na primeira vez que você tentar navegar em uma página no site da Web.


![Especificando uma URL Base para Sites FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: especificando uma URL Base para Sites FTP


## <a name="improvements-in-compilation"></a>Melhorias na compilação

Trabalhar com aplicativos Web no Visual Studio 2005 é muito mais rápida do que as versões anteriores. Isso se deve em nenhuma parte pequena alterações na arquitetura de compilação.

No Visual Studio 2002 e 2003, os aplicativos da Web foram compilados em um assembly principal que residem na pasta /bin. No Visual Studio 2005, foi adicionada a uma pasta de Code. Classes e outros códigos sem interface do usuário são adicionados à pasta de Code. Quando o Visual Studio compila o projeto, todos os arquivos na pasta do Code são compilados em um único arquivo App/_Code.dll. O resultado dessa alteração é que as compilações subsequentes são muito mais rápidas do que nas versões anteriores.

> [!NOTE]
> O utilitário de linha de comando do MSBuild também pode ser usado para criar aplicativos do ASP.NET. Essa ferramenta será abordada no módulo 9.


Outra melhoria de compilação é a nova opção de página de Build no menu compilar. Esse recurso permite que um desenvolvedor recompilar somente a página atual (juntamente com, do curso e dependências) para que as alterações podem ser compiladas mais rapidamente. Porque o c# não oferece a compilação em segundo plano para fins de atualização do IntelliSense, etc., eles serão beneficiados imensamente com esse recurso porque ele permitirá o IntelliSense para ser atualizada rapidamente por simplesmente recriar uma única página.

As propriedades de Build para um projeto permitem que você configurar o tipo de compilação que ocorre antes que a página de inicialização é executada. Os desenvolvedores podem optar por compilar apenas a página atual para que o Visual Studio pode iniciar a depuração de aplicativos mais rapidamente após as alterações de código.


![A ação de início da página de Build](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: A ação de início da página de Build


Outro ótimo aprimoramento para Visual Studio e a arquitetura do ASP.NET está na área de editar e continuar. No Visual Studio 2005, os desenvolvedores podem iniciar a depuração de um projeto e fazer alterações de código no projeto sem desanexar o depurador. Na verdade, literalmente, você pode iniciar a depuração de um projeto, adicione uma nova classe, adicione código à classe, adicione código para a página que cria uma nova instância da classe e executar um método da classe, tudo sem desanexar o depurador. Executar o novo código é literalmente tão fácil como atualizar o navegador!

Clique aqui para ver uma vídeo passo a passo da edição e continuar o recurso no Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


A robusta de editar e continuar a funcionalidade no ASP.NET 2.0 e o Visual Studio 2005 é devido a uma alteração de arquitetura para aplicativos ASP.NET. No ASP.NET 1. x, os aplicativos criados no Visual Studio 2002/2003 foram compilados em um assembly principal que foi armazenado na pasta /bin. Todas as classes, páginas, etc. para o aplicativo foram compiladas em uma DLL. Em seguida, no tempo de execução ASP.NET seria compilar todos os controles, marcação e código do ASP.NET em páginas e copie essas DLLs na pasta temporária ASP.NET.

No Visual Studio 2005 usando o ASP.NET 2.0, os modelos de dois compilação descrevem acima (uma para o Visual Studio) e outra para o ASP.NET no tempo de execução foram mesclados em um modelo comum de compilação. Isso significa que todos os problemas de compilação agora são capturados durante o estágio de desenvolvimento em vez de em tempo de execução. Ele também permite designer e o suporte ao IntelliSense para recursos, como controles de usuário e as páginas mestras.

Clique aqui para ver uma vídeo passo a passo de suporte de designer para controles de usuário.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Quando um controle de usuário é removido de uma página, o @Register diretiva permanece na marcação e devem ser removida manualmente para evitar erros do analisador, se o controle de usuário é excluído do site da Web.


Outro aperfeiçoamento no modelo de compilação do Visual Studio é o recurso de Publicar Site. Porque o recurso de publicação pré-compila um site da Web, os desenvolvedores podem aproveitar o melhor desempenho de não precisar compilar tudo sob demanda. Ele também pré-compila todo o código-fonte na pasta do Code em uma DLL para que nenhum código-fonte deve ser implantado.


![A caixa de diálogo Publicar Web Site](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: caixa de diálogo Publicar Web Site


> [!NOTE]
> O utilitário de aspnet/_compile.exe também pode ser usado para pré-compilar um aplicativo Web ASP.NET. Essa ferramenta será abordada no módulo 9.


Quando você publicar um site da Web, os arquivos pré-compilado é armazenados na pasta Temporary ASP.NET Files, conforme mostrado abaixo. Arquivos com um *Compiled* extensão de arquivo são arquivos XML que definem as dependências para DLLs em particulares. Os controles de formulário da Web ou de usuário são compilados em DLLs aleatórias que começam com *aplicativo /_Web /_*.

Se você deixar o *permitir que este site pré-compilado seja atualizado* caixa de seleção marcada, a marcação dentro de seus controles de formulários da Web e o usuário não poderá ser previamente compilada em uma DLL, permitindo que você faça alterações após a implantação. Se você desejar bloquear a marcação para que não são permitidas alterações ao conteúdo implantado, desmarque essa caixa.

O *opção Use fixed assemblies de página única e nomenclatura* caixa de seleção permite que você desabilite a compilação em lote para que cada página é compilada em um assembly de nome fixo. Deixar essa caixa desmarcada permite que você aproveite a compilação em lotes.

O *habilitar nomes fortes em assemblies de pré-compilado* caixa de seleção permite nome forte dos assemblies pré-compilados.

> [!NOTE]
> No ASP.NET 1. x, assemblies de nome forte precisava ser instalado no Cache de Assembly Global (GAC). No ASP.NET 2.0, não é necessário instalar assemblies de nome forte no GAC.


![Um arquivos pré-compiladas de aplicativos do ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: um arquivos pré-compiladas de aplicativos do ASP.NET


> [!NOTE]
> No aplicativo, não houve nenhum arquivo Web. config. Se tivesse sido, ele poderia ter sido chamado *PrecompiledApp.config* processo do site depois de publicar na Web.


## <a name="improvements-in-deployment"></a>Aprimoramentos na implantação

Como com o Visual Studio 2002 e 2003, o Visual Studio 2005 oferece um recurso de copiar projeto. No entanto, o recurso foi reforçado no Visual Studio 2005 e agora é chamado de Copy Web Site.

A caixa de diálogo Copy Web Site é dividida em um quadro à esquerda e um quadro à direita. O quadro à esquerda é chamado de Site de origem e o quadro à direita é chamado de Site remoto. Uma coisa que pode confundir alguns desenvolvedores é que o site exibido no quadro à direita não é necessariamente um site remoto. Pode ser um site no sistema de arquivos local ou na instância local do IIS. Além disso, o site exibido no quadro esquerdo não é necessariamente o site de origem porque a caixa de diálogo permite que você publicar a partir do site remoto *para* site de origem.

Se você estiver copiando um projeto para um site remoto, o site deve ter as extensões de servidor do FrontPage instalado nele. Se isso não acontecer, você precisará se conectar usando o FTP. Por outro lado, se você estiver copiando um projeto para a instância local do IIS, FrontPage Server Extensions não são necessários.

> [!NOTE]
> Se você tentar criar um novo site na instância local do IIS e as extensões de servidor do FrontPage 2002 estiverem instalados, você receberá uma mensagem de erro informando que a criação de sites da Web não é suportado em um servidor do SharePoint. Nesse caso, você tem a opção de instalar as extensões de servidor do FrontPage 2000 ou remover as extensões FrontPage Server Extensions.


Clique aqui para obter uma explicação de vídeo do recurso Copy Web Site.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Melhorias na depuração

Há quatro principais melhorias na depuração no Visual Studio 2005.

- Depurando localmente como um administrador não é possível fora da caixa.
- O atributo de depuração para o elemento de compilação agora é false por padrão.
- Instalação e configuração de depuração remota é mais fácil do que antes.
- Agora você pode depurar um site da Web aberto por meio de um local FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuração como não-administrador

A adição do ASP.NET Development Server permite que os usuários não administradores facilmente depurar os aplicativos ASP.NET prontos. Quando um aplicativo ASP.NET em execução no sistema de arquivos local é depurado, o Visual Studio inicia o ASP.NET Development Server sob o contexto do usuário conectado. Esse usuário pode, em seguida, depurar esse aplicativo sem qualquer configuração adicional.

## <a name="debug-is-false-by-default"></a>Depuração é False por padrão

No ASP.NET 1. x, o *debug* atributo na *compilação* elemento do arquivo Web. config foi definido como *true* por padrão. Ele sempre tenha sido recomendado que os desenvolvedores defina esse atributo como *false* antes de implantar um aplicativo para produção, mas porque a maioria dos desenvolvedores não compreender totalmente as consequências de deixar o atributo definido como True, eles simplesmente da esquerda-lo como-está.

O problema mais grave com o atributo de depuração de ter definido como true é que ela desabilita o modelo de compilação em lotes ASP.NETs. Portanto, cada página é compilada em uma DLL separada. Se uma Web aplicativo consiste em milhares de páginas (não nunca antes vistos de forma alguma), isso significa que várias milhares DLLs de pequenas serão criados por esse aplicativo. Embora essas DLLs estão tamanho pequenos, eles não são carregados em qualquer local específico na memória. Portanto, elas causam fragmentação na memória do sistema e podem contribuir com a OutOfMemoryException ocorrências.

No ASP.NET 2.0, o atributo é definido como false por padrão. Como você já viu, quando um desenvolvedor depura um aplicativo ASP.NET no Visual Studio 2005, ele será solicitado a adicionar um arquivo Web. config com depuração habilitada. Isso resulta em desvantagens mesmas que estavam presentes no ASP.NET 1. x, mas agora o desenvolvedor claramente é avisado de que o atributo deve ser redefinido como false antes de mover o aplicativo em produção.

## <a name="remote-debugging-setup-and-configuration"></a>Instalação e configuração de depuração remota

No Visual Studio 2002/2003, a depuração remota contava com o Gerenciador de depuração da máquina (mdm.exe) e o processo de vs7jit.exe. Por causa disso, solução de problemas de depuração remotos geralmente era uma caixa preta para os clientes e geralmente não era muito melhor para o Atendimento Microsoft.

Visual Studio 2005 remove a confiança nos processos de mdm.exe e vs7jit.exe. Em vez disso, ele agora usa o serviço do Monitor de depuração remota (msvsmon.exe).

O requisito para a depuração remota no Visual Studio 2005 é bastante simple. Você precisará executar msvsmon.exe no servidor remoto antes da depuração. Você pode instalar o Monitor de depuração remota do CD do Visual Studio ou você pode simplesmente executar msvsmon.exe de um compartilhamento sem instalar nada em todos os no servidor Web.

Quando você executa msvsmon.exe, é provável que ele vai reclamar sobre portas sendo bloqueadas para a depuração remota. Felizmente, você pode desbloquear as portas diretamente de dentro da caixa de diálogo de aviso facilmente conforme mostrado abaixo.


![Notificação de que o Firewall do Windows está bloqueando a depuração remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notificação de que o Windows Firewall está bloqueando a depuração remota


Quando você tiver desbloqueado as portas necessárias para a depuração, você verá o Monitor de depuração remota conforme mostrado abaixo. Essa interface, você pode monitorar as conexões e alterar permissões de depuração com facilidade.


![O Monitor de depuração remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: O Monitor de depuração remota


Também é possível depurar remotamente um aplicativo Web aberto por meio de FTP. As etapas são as mesmas que as abordadas anteriormente. No entanto, você precisará especificar uma URL base para procurar o projeto FTP, conforme descrito neste módulo.

## <a name="lab-2"></a>Laboratório 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuração remota com o Visual Studio 2005

Este laboratório orientará você por meio de depuração remota com o Visual Studio 2005.

Clique aqui para obter uma explicação de vídeo deste laboratório.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Este laboratório requer que você tenha duas máquinas, um executando o Visual Studio 2005 e o outro em execução do IIS 5 ou superior.

1. Abra o Visual Studio 2005 e crie um novo site no servidor remoto.

> [!NOTE]
> Você pode criar o site da Web em uma instância remota do IIS ou por meio de FTP.


1. Do servidor Web remoto, localize msvsmon.exe na máquina de desenvolvimento usando um caminho UNC e executá-lo.  
 O local padrão para msvsmon.exe é //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Se for solicitado a desbloquear portas para depuração remota, faça isso.
3. Da máquina de desenvolvimento, abra o code-behind para default. aspx e defina um ponto de interrupção no método de carregar.
4. Inicie a depuração da máquina de desenvolvimento.

Você deverá atingir o ponto de interrupção conforme o esperado.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Como weve já discutido, o Visual Studio 2005 é fornecido com um servidor de Web chamado o ASP.NET Development Server. (O ASP.NET Development Server é às vezes chamado como Cassini.) Esse servidor Web é um meio conveniente de navegar e depurar aplicativos Web em execução no sistema de arquivos.

O ASP.NET Development Server é um servidor Web restrito. Ele não permite conexões remotas, ela não permite todas as solicitações de qualquer usuário que não seja o usuário que iniciou o servidor Web. Ele também não tem a capacidade de servir as páginas ASP. Somente os recursos do ASP.NET e recursos HTML (incluindo imagens, arquivos CSS, etc.) são atendidos.

O ASP.NET Development Server podem ser iniciado por meio da linha de comando, executando o arquivo WebDev.WebServer.exe localizado em c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. A caixa de diálogo a seguir exibe os parâmetros que estão disponíveis.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**


> [!NOTE]
> Não há suporte para o ASP.NET Development Server quando iniciado explicitamente por meio da linha de comando.
