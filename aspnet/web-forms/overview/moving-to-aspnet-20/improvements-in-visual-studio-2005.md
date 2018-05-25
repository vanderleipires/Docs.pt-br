---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Melhorias no Visual Studio 2005 | Microsoft Docs
author: microsoft
description: O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de melhorias para projetos da Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a>Melhorias no Visual Studio 2005
====================
por [Microsoft](https://github.com/microsoft)

> O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de melhorias para projetos da Web.


O Visual Studio 2005 fornece aos desenvolvedores de aplicativos Web uma longa lista de melhorias para projetos da Web. Mesmo avançado como o Visual Studio .NET 2002 e 2003 são, houve muitas reclamações a maneira como os projetos da Web foram manipulados. O Visual Studio 2005 adiciona um número significativo de novos recursos para lidar com essas reclamações. Para aqueles que preferem a maneira como o Visual Studio .NET 2003 tratados compilação de aplicativos da Web, consulte [projetos de aplicativo Web](https://go.microsoft.com/fwlink/?LinkId=57870).

Neste módulo, também abrangem melhorias na criação do projeto da Web, gerenciamento e desenvolvimento. Em um módulo posterior, também abrangem melhorias na criação de projetos da Web e implantá-los.

## <a name="frontpage-server-extensions"></a>Extensões de servidor do FrontPage

O Visual Studio .NET 2002 e 2003 necessário extensões na caixa para criar ou criar projetos da Web. Os desenvolvedores tinha uma escolha entre dois modos de acesso diferentes (extensões de servidor do FrontPage ou arquivo de modo de acesso), ambos usados extensões para executar tarefas como definir a raiz do aplicativo no IIS, etc.

O Visual Studio 2005 remove a dependência de extensões para projetos locais. Agora, o Visual Studio 2005 acessa a metabase do IIS diretamente, em vez de usar as extensões de servidor do FrontPage. O Visual Studio 2005 também adiciona suporte para FTP, que permite o acesso remoto do projeto sem a necessidade de extensões de servidor do FrontPage.

Para os desenvolvedores que desejam usar extensões em seus projetos, a opção ainda está disponível. No entanto, com base nos comentários fortes da comunidade de desenvolvedores do ASP.NET, não é um requisito.

> [!NOTE]
> Extensões de servidor do FrontPage ainda são necessárias para a criação do projeto remoto, abrir, etc.


## <a name="aspnet-development-server"></a>ASP.NET Development Server

O Visual Studio 2005 vem com um novo servidor Web chamado ASP.NET Development Server. (Esse servidor Web era conhecido anteriormente como Cassini).

Há vários benefícios do servidor de desenvolvimento do ASP.NET.

- Agora é possível para administradores desenvolver e depurar em um servidor Web.
- O ASP.NET Development Server mapeia dinamicamente os diretórios virtuais em qualquer local no sistema de arquivos, permitindo a locais de projeto flexível.
- Os usuários no Windows XP Professional que já estiver usando o IIS agora será capazes de criar novos aplicativos Web que não afetam a estrutura de pasta ou arquivo do seu Site padrão no IIS.

Nenhuma configuração especial é necessária para aproveitar o ASP.NET Development Server. Quando um projeto da Web que é hospedado no sistema de arquivos é depurado ou procurado, o Visual Studio 2005 iniciará automaticamente uma instância do servidor de desenvolvimento ASP.NET em uma porta aleatória para atender à solicitação.

Obter mais informações serão abordadas no servidor de desenvolvimento ASP.NET neste módulo.

## <a name="improved-file-management"></a>Gerenciamento de arquivos aprimorado

No Visual Studio 2002 e 2003, um arquivo de projeto (. vbproj para VB.NET) e. csproj c# informações armazenadas em todos os arquivos no aplicativo Web. A exibição do Gerenciador de soluções baseia-se as informações do arquivo no arquivo de projeto. Por isso, o Gerenciador de soluções geralmente exibirá informações precisas em casos onde editores externos foram usados. O Visual Studio 2002 e 2003 seria geralmente substituir as alterações de arquivo ou não exibir a versão mais recente dos arquivos.

Visual Studio 2005 imediatamente faz com que o arquivo de projeto. Em vez disso, ele lê as informações de arquivo e pasta diretamente do disco, resultando em uma exibição precisa dos arquivos em seu projeto. Porque a pasta de referências no Visual Studio 2002 e 2003 não representa uma pasta real em seu aplicativo Web, o Visual Studio 2005 também remove a pasta referências no Gerenciador de soluções. Para acessar as referências para o seu projeto no Visual Studio 2005, você deve usar as páginas de propriedades para o projeto.

## <a name="creating-web-projects"></a>Criando projetos da Web

Os desenvolvedores da Web tem muitas opções novas disponíveis para a criação do projeto no Visual Studio 2005. Sites da Web agora podem ser criados em qualquer lugar no sistema de arquivos e, em seguida, pode ser depurados ou pesquisado usando o novo servidor de desenvolvimento do ASP.NET. Os desenvolvedores também podem criar novos sites da Web usando FTP.

Clique aqui para exibir um vídeo passo a passo de criação de projetos Web no Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Projetos do sistema de arquivos

Como você viu no passo a passo de vídeo, você pode escolher criar sites da Web no sistema de arquivos no computador local ou em um local remoto por meio de um compartilhamento de arquivos. Sites da Web que são criadas no sistema de arquivos são acessados e depurado usando o ASP.NET Development Server.

> [!NOTE]
> O ASP.NET Development Server pode causar alguma confusão para os clientes. Se um projeto da Web é criado no sistema de arquivos na estrutura de diretório IISs (por exemplo, c: inetpub/wwwroot), o site ainda será ser navegado por meio do servidor de desenvolvimento ASP.NET quando iniciado a partir do Visual Studio 2005. Portanto, qualquer configuração de IIS (ou seja, métodos de autenticação) não é aplicável.


O projeto da web padrão também remove muito da sobrecarga por inclui apenas uma página Default.aspx, arquivo default.cs e uma pasta de Data. O Web. config e pastas especiais (por exemplo, Code) são adicionadas como eles são necessários. O projeto da web inclui apenas os arquivos e pastas que você precisa.

### <a name="http-projects"></a>Projetos HTTP

Projetos HTTP podem ser projetos que são criados em um site da Web do IIS local ou em um site remoto. O local do projeto padrão é `http://localhost`. Se você clicar no botão Procurar, há duas opções de HTTP: IIS Local e o Site remoto. A principal diferença nessas duas opções é o método no qual as informações do site da web são exibidas na caixa de diálogo Escolher local e em como os arquivos são copiados para o servidor Web.

A opção de Local IIS lê as informações do site da metabase no computador local e os arquivos são copiados usando o sistema de arquivos. A opção de Site remoto usa as extensões de servidor do FrontPage e as informações do site e os arquivos são copiados usando HTTP e chamadas de RPC de extensões de servidor do FrontPage.

> [!NOTE]
> O arquivo de vs###/_tmp.htm e get/_aspx/_ver.aspx não são usados para determinar as informações de versão.


A opção de HTTP padrão é o Local do IIS. Essa opção lê a Metabase do IIS para determinar quais sites estão disponíveis e o local no qual criar o conteúdo. Você pode selecionar uma pasta diferente ou um diretório virtual, selecionando-o na exibição de árvore. Você pode também criar um novo diretório virtual, marcar pastas como aplicativos, bem como excluir diretórios virtuais existentes nessa caixa de diálogo.


![A escolha da caixa de diálogo local](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: A escolher a caixa de diálogo local


Ao contrário em versões anteriores do Visual Studio, se você verificar o **usar Secure Sockets Layer** caixa de seleção e o certificado SSL não coincide com a URL que você está pesquisando, você verá uma caixa de diálogo alerta de segurança, perguntando se você faria Deseja continuar. Usando o Visual Studio .NET 2003, se o certificado não era uma correspondência, criando o projeto falhará.


![Alerta certificado SSL sobre segurança](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: alerta de segurança sobre o certificado SSL


### <a name="note-on-host-headers"></a>Observação sobre cabeçalhos de Host

Se você estiver criando um aplicativo da Web em um site associado a um IP específico, você precisará garantir que um cabeçalho de host é configurado. Caso contrário, o Visual Studio criará o site em `http://localhost`, mas o endereço IP não será interpretado corretamente quando o site é pesquisado ou depurado de dentro do IDE.

Se você selecionar a opção de Site remoto, a caixa de diálogo é alterado para permitir que você insira a URL de destino para o novo site. Essa URL deve ser em um servidor que tem o FrontPage Server Extensions habilitado. Se você quiser trabalhar com seu servidor Web local usando as extensões de servidor do FrontPage, você pode usar a opção de Site remoto e especifique uma URL de local.


![Criar um Site da Web em um servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: Criando um Site da Web em um servidor remoto


Ao criar um aplicativo em um local remoto por meio de SSL, se o certificado SSL não corresponder, a caixa de diálogo de confirmação é ligeiramente diferente do que a caixa de diálogo exibida ao usar a opção do IIS Local.


![O alerta de segurança do Site remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: O alerta de segurança do Site remoto


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

O Visual Studio 2005 apresenta a opção para criar sites da Web por meio de FTP. Quando você usar essa opção, o IDE cria os arquivos localmente na pasta temp de usuários e, em seguida, usa o FTP para mover os arquivos para o local de FTP.

> [!NOTE]
> O local da pasta temporária é c: / Documents and Settings /&lt;usuário&gt;/Local Temp/configurações/VWDWebCache/&lt;servidor&gt;/_&lt;nome do aplicativo&gt;


Ao usar a opção de FTP, você verá uma caixa de diálogo Escolher local. Você pode inserir as informações de conexão de FTP necessárias nesta caixa de diálogo, conforme mostrado abaixo.


![A escolha da caixa de diálogo de local de FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: A escolher a caixa de diálogo de local de FTP


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratório: Configuração FTP do site e criar um projeto

As etapas a seguir configuram o site FTP para que um usuário tem um local que só eles podem carregar por meio de FTP.

### <a name="install-the-ftp-service"></a>Instalar o serviço de FTP

1. Abrir Adicionar ou remover programas, selecione Adicionar/remover componentes do Windows
2. Selecione os serviços de informações da Internet (servidor de aplicativos no Windows 2003) e clique em **detalhes**.
3. Verificar **serviço de protocolo de transferência de arquivo (FTP)** e clique em **Okey**.
4. Clique em **próximo** para instalar o serviço FTP.

### <a name="create-a-new-folder-for-content"></a>Crie uma nova pasta de conteúdo

1. No Windows Explorer, crie uma nova pasta chamada **User1** dentro de c: inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configure as pastas e permissões nas pastas.

1. Abra o snap-in de serviços de informações da Internet em Ferramentas administrativas. Agora, você terá uma pasta de Sites FTP sob o nó de nome do computador.
2. Expanda **Sites FTP**.
3. Com o botão direito o **Site FTP padrão**, selecione **novo**, em seguida, **Diretório Virtual**, em seguida, clique em **próximo**.
4. Digite **User1** para o nome do diretório virtual e clique **próximo**.
5. Digite **c: / inetpub/wwwroot/User1** para o caminho e clique **próximo**.
6. Clique em **próximo** e **concluir** para concluir o assistente.
7. Clique com botão direito do **User1** diretório virtual no Site FTP padrão e selecione **propriedades**.
8. Verifique o **gravar** caixa de seleção e clique em **Okey** para fechar a caixa de diálogo.
9. Clique com botão direito **Site FTP padrão** e selecione **propriedades**.
10. Sobre o **contas de segurança** guia, desmarque a opção **permitir conexões anônimas**.
11. Clique em **Sim** na caixa de diálogo perguntando se deseja continuar.
12. Clique em **Okey** para fechar a caixa de diálogo.
13. Expanda o **Default Web Site** sob o **Sites da Web** nó.
14. Clique com botão direito do **User1** diretório e selecione **propriedades**
15. No **configurações do aplicativo** seção, clique em **criar** para marcar a pasta como um aplicativo.
16. Clique em **Okey** para fechar a caixa de diálogo.
17. Feche o snap-in de serviços de informações da Internet.

### <a name="create-web-project"></a>Criar projeto da web

1. Abra o Visual Studio 2005.
2. Do **arquivo** menu, selecione **novo Site**.
3. No **local** lista suspensa, selecione **FTP**.
4. Clique em **Procurar**.
5. Digite **localhost** no **Server** caixa de texto.
6. Digite **User1** na caixa de texto diretório.
7. Clique em **abrir**. O local de FTP será inserido na caixa de diálogo Novo Site da Web.
8. Clique em **OK**.
9. Desmarque **Anonymous Logon** na caixa de diálogo logon de FTP, insira suas credenciais e clique em **Okey**.
10. Qual é a URL para o projeto? (A URL para o projeto será exibida no Gerenciador de soluções.)
11. Do **criar** menu, selecione **Compilar Site** ou **compilar solução**.
12. Clique duas vezes em Default.aspx no Gerenciador de soluções e selecione **exibir no navegador**.
13. Na caixa de diálogo necessário URL do Site da Web, digite `http://localhost/user1` para a URL e clique em **Okey**.

> [!NOTE]
> Se você receber um erro indicando a impossibilidade de carregar o tipo /_Default, certifique-se de que você está executando o ASP.NET 2.0 no seu site da Web e não uma versão anterior. Você pode fazer isso na guia ASP.NET nos serviços de informações da Internet.


## <a name="opening-web-projects"></a>Abrindo projetos da Web

Abrir projetos da Web é semelhante à criação de projetos. As seções a seguir chamar áreas ficar de olho-out para enquanto estiver trabalhando no IDE. Ele também aborda a trabalhar com projetos da Web usando HTTP e FTP.

Para abrir um projeto da Web, selecione Abrir o Site da Web no menu arquivo. Você será solicitado com a mesma caixa de diálogo Escolher local abordada anteriormente e ter as mesmas quatro opções disponíveis para você: Site remoto, IIS Local, FTP e sistema de arquivos.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de arquivos

Como indicado anteriormente neste módulo, o Visual Studio não usa um arquivo de projeto. Portanto, se você optar por abrir um site do sistema de arquivos, você realmente tem a opção de escolher qualquer pasta que desejar, mesmo se a pasta que você escolher não foi criada como um projeto Web inicialmente no Visual Studio. Por exemplo, você pode optar por abrir a pasta Meus documentos, como um site da Web e o Visual Studio Felizmente abri-lo e exibir os arquivos, conforme mostrado abaixo.


![Meus documentos abertos como um Site da Web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Meus documentos* aberto como um Site da Web


Porque somente o Visual Studio cria outros arquivos e pastas, quando necessário, sem arquivos ou pastas adicionais são adicionadas para o local em que você abrir. Um efeito colateral dessa arquitetura é que ela impede que você o aninhamento de sites da Web no sistema de arquivos. Por exemplo, considere a seguinte estrutura de diretório.

Projeto da Web em c: / Meuwebsite

Outro projeto da web em c: / Meuwebsite/aninhadas

Quando você abrir o site da Web no c/Meuwebsite, a pasta aninhadas aparecerá como uma subpasta do aplicativo.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Ao abrir sites da Web via HTTP, as configurações são lidas da metabase do IIS (IIS Local) ou usando o FrontPage Server Extensions (Site remoto). Se houver aplicativos web aninhadas, eles são exibidos também com um ícone que identifica como um aplicativo. Se você estiver familiarizado com a trabalhar com aplicativos web do FrontPage, o comportamento no Visual Studio 2005 é semelhante.

Embora o Visual Studio exibirá um ícone para aplicativos que estão aninhadas sob o aplicativo que está atualmente aberto no IDE, ele não permitirá que você expandir para ver seu conteúdo. No entanto, você pode clicar duas vezes neles para abri-los. Quando você fizer isso, você verá uma caixa de diálogo solicitando que você abrir o aplicativo web (e substitua a solução atualmente aberta) ou adicionar o aplicativo Web para sua solução atual.


![Clicando duas vezes em um ícone de aplicativo aninhada apresenta esta caixa de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: clicando duas vezes em um ícone de aplicativo aninhada apresenta esta caixa de diálogo


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Site FTP

Quando você abre um site por meio de FTP, os arquivos são todos copiados localmente para a pasta temp. O caminho completo para o local de armazenamento local é exibido no painel de propriedades do projeto e é criado usando o formato a seguir.

C: / Documents and Settings /&lt;usuário&gt;/Local Temp/configurações/VWDWebCache/&lt;servidor&gt;/_&lt;nome do aplicativo&gt;

Ao usar o FTP, Visual Studio precisará especificar a URL base para o seu projeto para que você pode procurar conforme mostrado abaixo. Se você não especificar uma URL base, Visual Studio solicitará para ele na primeira vez que você tentar navegar em uma página no site da Web.


![Especificando uma URL Base para Sites FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: especificando uma URL Base para Sites FTP


## <a name="improvements-in-compilation"></a>Melhorias na compilação

Trabalhar com aplicativos Web no Visual Studio 2005 é consideravelmente mais rápido do que as versões anteriores. Isso é nenhuma parte pequena devido às alterações na arquitetura de compilação.

No Visual Studio 2002 e 2003, os aplicativos da Web foram compilados em um assembly principal que reside na pasta /bin. No Visual Studio 2005, foi adicionada a uma pasta de Code. Classes e outro código de interface de usuário não são adicionadas à pasta de Code. Quando o Visual Studio compila o projeto, todos os arquivos na pasta do Code são compilados em um único arquivo App/_Code.dll. O resultado dessa alteração é que as compilações subsequentes são muito mais rápidas do que nas versões anteriores.

> [!NOTE]
> O utilitário de linha de comando do MSBuild também pode ser usado para criar aplicativos do ASP.NET. Essa ferramenta será abordada no módulo 9.


Outro aprimoramento de compilação é a nova opção de página Build no menu Build. Esse recurso permite que um desenvolvedor recriar apenas a página atual (juntamente com, do curso e dependências) para que as alterações podem ser compiladas mais rapidamente. Porque c# não oferece a compilação do plano de fundo para fins de atualização do IntelliSense, etc., eles serão beneficiadas imensamente desse recurso porque ele permitirá IntelliSense para ser atualizado rapidamente, simplesmente recriando uma única página.

As propriedades de compilação para um projeto permitem que você configure o tipo de compilação que ocorre antes da página de inicialização é executada. Os desenvolvedores podem optar por criar somente a página atual para que o Visual Studio pode iniciar a depuração de aplicativos mais rapidamente após as alterações de código.


![A ação de início da página de compilação](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: A ação de início da página de compilação


Outro aprimoramento excelente para Visual Studio e a arquitetura do ASP.NET está na área de editar e continuar. No Visual Studio 2005, os desenvolvedores podem iniciar a depuração de um projeto e fazer alterações de código no projeto sem desanexar o depurador. Na verdade, literalmente pode iniciar a depuração de um projeto, adicione uma nova classe, adicione código para essa classe, adicione o código para a página que cria uma nova instância da classe e executar um método da classe, tudo sem desanexar o depurador. Executar o novo código é literalmente mais fácil atualizar o navegador!

Clique aqui para ver um vídeo passo a passo de editar e continuar o recurso no Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


O robusto de edita e continuar a funcionalidade no ASP.NET 2.0 e o Visual Studio 2005 é devido a uma alteração estrutural para aplicativos ASP.NET. No ASP.NET 1. x, os aplicativos criados no Visual Studio 2002/2003 foram compilados em um assembly principal que foi armazenado na pasta /bin. Todas as classes, páginas, etc. para o aplicativo foi compilado em que uma DLL. Em tempo de execução, o ASP.NET, em seguida, seria compilar todos os controles, a marcação e o código ASP.NET em páginas e copiar as DLLs na pasta temporária do ASP.NET.

No Visual Studio 2005 usando o ASP.NET 2.0, os modelos de dois compilação descrevem acima (uma para o Visual Studio) e outra para o ASP.NET em tempo de execução foram mescladas em um modelo comum de compilação. Isso significa que todos os problemas de compilação agora são detectados durante a fase de desenvolvimento, em vez de em tempo de execução. Ele também permite para o designer e o suporte ao IntelliSense para recursos, como páginas mestras e controles de usuário.

Clique aqui para ver um vídeo passo a passo de suporte de designer para controles de usuário.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Quando um controle de usuário é removido de uma página, o @Register diretiva permanece na marcação e deve ser removida manualmente para evitar erros de analisador se o controle de usuário é excluído do site.


Outro aprimoramento no modelo de compilação do Visual Studio é o recurso Publicar Site. Porque o recurso de publicação pré-compila o site da Web, os desenvolvedores podem aproveitar o desempenho de não precisar compilar tudo sob demanda. Ele também pré-compila todo o código fonte na pasta do Code em uma DLL para que nenhum código-fonte deve ser implantado.


![A caixa de diálogo Publicar Web Site](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: A caixa de diálogo Publicar Web Site


> [!NOTE]
> O utilitário aspnet/_compile.exe também pode ser usado para pré-compilar um aplicativo Web ASP.NET. Essa ferramenta será abordada no módulo 9.


Quando você publicar um site da Web, os arquivos pré-compilado é armazenados na pasta arquivos temporários do ASP.NET, conforme mostrado abaixo. Arquivos com um *. Compiled* extensão de arquivo são arquivos XML que definem as dependências para DLLs específicos. Os controles de formulário da Web ou de usuário são compilados para DLLs aleatórios que começam com *aplicativo /_Web /_*.

Se você deixar o *permitir que este site pré-compilado seja atualizado* caixa de seleção marcada, a marcação dentro de seus controles de formulários da Web e o usuário não será pré-compilado em uma DLL que permitem fazer alterações após a implantação. Se você preferir bloquear a marcação para que não são permitidas alterações ao conteúdo implantado, desmarque essa caixa.

O *Use fixa assemblies de página única e nomenclatura* caixa de seleção permite que você desabilite a compilação em lote para que cada página é compilada em um assembly de nome fixo. Sair dessa caixa desmarcada permite aproveitar compilação em lote.

O *habilitar nomes fortes em assemblies pré-compilados* caixa de seleção permite nome forte seus assemblies pré-compilados.

> [!NOTE]
> No ASP.NET 1. x, assemblies de nomes fortes precisava ser instalado no Cache de Assembly Global (GAC). No ASP.NET 2.0, não é necessário para instalar assemblies de nome forte no GAC.


![Um arquivos de pré-compilados de aplicativos ASP.NET](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: um arquivos de pré-compilados de aplicativos ASP.NET


> [!NOTE]
> No aplicativo acima, não havia nenhum arquivo Web. config. Se tivesse sido, ele poderia ter sido chamado *Precompiledapp* depois de publicar Web sites processo.


## <a name="improvements-in-deployment"></a>Melhorias na implantação

Como com o Visual Studio 2002 e 2003, o Visual Studio 2005 oferece um recurso de cópia de projeto. No entanto, o recurso foi reforçado no Visual Studio 2005 e agora é chamado de Site da Web de cópia.

A caixa de diálogo do Site da Web de cópia é dividida em um quadro à esquerda e um quadro direito. O quadro esquerdo é chamado de Site de origem e o quadro direito é chamado de Site remoto. Uma coisa que pode confundir alguns desenvolvedores é que o site exibido no quadro à direita não é necessariamente um site remoto. Pode ser um site no sistema de arquivos local ou na instância local do IIS. Além disso, o site exibido no quadro esquerdo não é necessariamente o site de origem porque a caixa de diálogo permite que você publicar do site remoto *para* o site de origem.

Se você estiver copiando um projeto para um site remoto, esse site deve ter as extensões de servidor do FrontPage instalado nele. Se não estiver, você precisará se conectar usando o FTP. Por outro lado, se você estiver copiando um projeto para a instância local do IIS, FrontPage Server Extensions não são necessários.

> [!NOTE]
> Se você tentar criar um novo site na instância local do IIS e as extensões de servidor do FrontPage 2002 estiverem instalados, você receberá uma mensagem de erro informando que a criação de sites da Web não é suportado em um servidor do SharePoint. Nesse caso, você tem a opção de instalar as extensões de servidor do FrontPage 2000 ou remover extensões de servidor do FrontPage.


Clique aqui para uma orientação em vídeo do recurso de cópia da Web Site.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Melhorias na depuração

Há quatro principais melhorias na depuração no Visual Studio 2005.

- Depurando localmente como um administrador não é possível fora da caixa.
- O atributo de depuração para o elemento de compilação agora é false por padrão.
- Instalação e configuração de depuração remota é mais fácil do que antes.
- Agora você pode depurar um site da Web aberto por meio de um local FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuração como não-administrador

A adição do servidor de desenvolvimento do ASP.NET permite que não administradores depurem facilmente aplicativos ASP.NET imediatamente. Quando um aplicativo ASP.NET em execução no sistema de arquivos local é depurado, o Visual Studio inicia o ASP.NET Development Server sob o contexto do usuário conectado. Esse usuário, em seguida, poderá depurar o aplicativo sem qualquer configuração adicional.

## <a name="debug-is-false-by-default"></a>Depuração é False por padrão

No ASP.NET 1. x, o *depurar* atributo o *compilação* elemento do arquivo Web. config foi definido como *true* por padrão. Ele sempre tenha sido recomendado que os desenvolvedores definir esse atributo para *false* antes de implantar um aplicativo para produção, mas porque a maioria dos desenvolvedores não entender as consequências de deixar o atributo para depuração True, eles simplesmente da esquerda como-é.

O problema mais grave com o atributo de depuração que tenha definido como true é que ele desabilita o modelo de compilação em lote ASP.NETs. Portanto, cada página é compilada em uma DLL separada. Se uma Web aplicativo consiste em milhares de páginas (não incomum de por qualquer meio), o que significa que várias DLLs pequenos milhar serão criados por esse aplicativo. Embora essas DLLs estão tamanho pequenos, elas não são carregadas em qualquer local específico na memória. Portanto, eles causam fragmentação na memória do sistema e podem contribuir com OutOfMemoryException ocorrências.

No ASP.NET 2.0, o atributo de depuração é definido como false por padrão. Como já vimos, quando um desenvolvedor depura um aplicativo ASP.NET no Visual Studio 2005, ele serão solicitados a adicionar um arquivo Web. config com depuração habilitada. Isso gera as mesmo desvantagens que estavam presentes no ASP.NET 1. x, mas agora o desenvolvedor claramente é avisado de que o atributo deve ser redefinido como false antes de mover o aplicativo para produção.

## <a name="remote-debugging-setup-and-configuration"></a>Instalação e configuração de depuração remota

Depuração remota no Visual Studio 2002/2003, dependia Machine Debug Manager (mdm.exe) e o processo de vs7jit.exe. Por isso, solução de problemas de depuração remotos geralmente era uma caixa preta para clientes e geralmente não era muito melhor para serviços de suporte.

O Visual Studio 2005 remove a dependência de processos de mdm.exe e vs7jit.exe. Em vez disso, ele agora usa o serviço do Monitor de depuração remoto (msvsmon.exe).

O requisito para a depuração remota no Visual Studio 2005 é muito simple. Você precisa executar o msvsmon.exe no servidor remoto antes da depuração. Você pode instalar o Monitor de depuração remota do CD do Visual Studio ou você pode simplesmente executar msvsmon.exe de um compartilhamento sem instalar nada em todos os no servidor Web.

Quando você executa o msvsmon.exe, é provável que ele emitirá um aviso sobre as portas que estão sendo bloqueadas para depuração remota. Felizmente, você poderá desbloquear as portas da direita na caixa de diálogo de aviso facilmente conforme mostrado abaixo.


![Notificação de que o Firewall do Windows está bloqueando a depuração remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notificação de que o Firewall do Windows está bloqueando a depuração remota


Depois que você tiver desbloqueado as portas necessárias para a depuração, você verá o Monitor de depuração remota conforme mostrado abaixo. Essa interface, você pode monitorar as conexões e alterar permissões de depuração facilmente.


![O Monitor de depuração remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: O Monitor de depuração remota


Também é possível depurar remotamente um aplicativo Web aberto por meio de FTP. As etapas são as mesmas que as abordadas anteriormente. No entanto, você precisará especificar a URL base para procurar o projeto FTP, conforme descrito neste módulo.

## <a name="lab-2"></a>Laboratório 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuração remota com o Visual Studio 2005

Este laboratório irá orientá-lo por meio de depuração remota com o Visual Studio 2005.

Clique aqui para obter uma explicação de vídeo deste laboratório.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Abra vídeo de tela inteira](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Este laboratório requer que você tenha duas máquinas, uma execução Visual Studio 2005 e o outro em execução do IIS 5 ou superior.

1. Abra o Visual Studio 2005 e crie um novo site no servidor remoto.

> [!NOTE]
> Você pode criar o site da Web em uma instância remota do IIS ou por meio de FTP.


1. Do servidor Web remoto, localize o msvsmon.exe no computador de desenvolvimento usando um caminho UNC e executá-lo.  
 O local padrão para msvsmon.exe é //server/c$/Program arquivos para o Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Se você precisará desbloquear portas para depuração remota, faça isso.
3. Da máquina de desenvolvimento, abra o code-behind para default. aspx e defina um ponto de interrupção no método de carga.
4. Inicie a depuração da máquina de desenvolvimento.

Você deve atingir o ponto de interrupção conforme o esperado.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Como weve já discutido, Visual Studio 2005 vem com um servidor Web chamado de servidor de desenvolvimento do ASP.NET. (O servidor de desenvolvimento ASP.NET é também conhecido como Cassini.) Esse servidor Web é uma maneira conveniente de procurar e depurar aplicativos da Web em execução no sistema de arquivos.

O servidor de desenvolvimento ASP.NET é um servidor da Web restrito. Ele não permite conexões remotas, ele não permite todas as solicitações de qualquer usuário que não seja o usuário que iniciou o servidor Web. Ele também não tem a capacidade de servir páginas ASP. Somente os recursos ASP.NET e recursos HTML (incluindo imagens, arquivos CSS, etc.) são atendidos.

O ASP.NET Development Server pode ser iniciado por meio da linha de comando executando o arquivo WebDev.WebServer.exe localizado em c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. A caixa de diálogo a seguir exibe os parâmetros que estão disponíveis.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**


> [!NOTE]
> Não há suporte para o servidor de desenvolvimento ASP.NET quando iniciado explicitamente por meio da linha de comando.
