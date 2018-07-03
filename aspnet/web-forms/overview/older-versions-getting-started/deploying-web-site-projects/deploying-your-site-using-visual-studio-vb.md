---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Implantando o Site usando o Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: Visual Studio inclui ferramentas para implantar um site. Saiba mais sobre essas ferramentas neste tutorial.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 127f051d9e9e2e4bd5b180185846ebd67586f41d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402675"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Implantando o Site usando o Visual Studio (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio inclui ferramentas para implantar um site. Saiba mais sobre essas ferramentas neste tutorial.


## <a name="introduction"></a>Introdução

O tutorial anterior analisamos como implantar um aplicativo simples da web ASP.NET em um provedor de host da web. Especificamente, o tutorial mostrado como usar um cliente FTP como o FileZilla para transferir os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. Visual Studio também oferece as ferramentas internas para facilitar a implantação em um provedor de host da web. Este tutorial examina duas dessas ferramentas: a ferramenta Copy Web Site, onde você pode mover os arquivos de e para um servidor web remoto usando o FTP ou o FrontPage Server Extensions; e a ferramenta de publicação, que copia todo o site para um local especificado.


> [!NOTE]
> Outras ferramentas relacionadas à implantação oferecidas pelo Visual Studio incluem [projetos de instalação da Web](https://msdn.microsoft.com/library/wx3b589t.aspx) e [Web Deployment Projects](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Projetos de instalação da Web o conteúdo de um site e informações de configuração em um único arquivo MSI do pacote. Essa opção é mais útil para sites que são implantados em uma intranet ou para as empresas que vendem um aplicativo web pré-empacotados que os clientes instalem em seus próprios servidores web. O Add-In de projetos de implantação de Web é que um Visual Studio Add-In que facilita a especificação de diferenças de configuração entre compilações para ambientes de produção e ambientes de desenvolvimento. Projetos de instalação da Web não são abordados nesta série de tutoriais; Projetos de implantação da Web são resumidos na [ *diferenças de configuração comuns entre desenvolvimento e produção* ](common-configuration-differences-between-development-and-production-vb.md) tutorial.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Implantando o Site usando a ferramenta Copy Web Site

Ferramenta de Copy Web Site do Visual Studio é semelhante em funcionalidade a um cliente FTP autônomo. Em resumo, a ferramenta Copy Web Site permite que você se conecte a um site remoto através de FTP ou o FrontPage Server Extensions. Semelhante à interface do usuário do FileZilla, a interface do usuário Copy Web Site consiste em dois painéis: o painel esquerdo lista os arquivos locais, enquanto o painel direito lista esses arquivos no servidor de destino.

> [!NOTE]
> A ferramenta Copy Web Site só está disponível para projetos de Site. Visual Studio oferecem essa ferramenta quando você estiver trabalhando com um projeto de aplicativo Web.


Vamos dar uma olhada em como usar a ferramenta Copy Web Site para publicar o aplicativo resenha de livro para produção. Porque a ferramenta Copy Web Site só funciona com projetos que usam o modelo de projeto de Site que só pode examinar usando essa ferramenta com o projeto BookReviewsWSP. Abra o projeto.

Inicie o projeto da ferramenta Copy Web Site clicando no ícone de Copy Web Site no Gerenciador de soluções (esse ícone é circulado na Figura 1); Como alternativa, você pode selecionar a opção Copy Web Site no menu do site. Qualquer uma das abordagens inicia a interface do usuário Copy Web Site mostrada na Figura 1; apenas o painel esquerdo na Figura 1 é popular porque ainda temos para se conectar a um servidor remoto.


[![Interface do usuário da ferramenta do Site da Web a cópia é dividido em dois painéis](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Figura 1**: Interface a ferramenta Copy Web Site do usuário é dividido em dois painéis ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image3.png))


Para implantar nosso site, precisamos primeiro se conectar ao provedor de host da web. Clique no botão Conectar na parte superior da interface do usuário Copy Web Site. Isso exibe a caixa de diálogo Abrir Site mostrada na Figura 2.

Você pode se conectar ao site de destino, selecionando uma das quatro opções da esquerda:

- **Sistema de arquivos** -Selecione esta opção para implantar seu site em um pasta ou compartilhamento de rede acessível do computador.
- **IIS local** -use esta opção para implantar o site para o servidor web do IIS instalado no seu computador.
- **FTP Site** -se conectar a um site remoto usando o FTP.
- **Site remoto** -conecte a um site remoto usando as extensões FrontPage Server Extensions.

A maioria dos provedores de host da web oferecem suporte a FTP, mas menos oferecem suporte à extensão de servidor do FrontPage. Por esse motivo, selecionamos a opção do Site de FTP e, em seguida, inserir as informações de conexão, conforme mostrado na Figura 2.


[![Especifique o site de destino](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Figura 2**: especifique o site de destino ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Depois de se conectar, a ferramenta Copy Web Site carrega os arquivos no site remoto no painel direito e indica o status de cada arquivo: novos, excluídos, alterado ou não alterado. Você pode copiar um arquivo do site local para o site remoto, ou vice-versa um.

Vamos adicionar uma nova página ao projeto BookReviewsWSP e, em seguida, implantá-lo para que possamos ver a ferramenta Copy Web Site em ação. Criar uma nova página ASP.NET no Visual Studio no diretório raiz chamado `Privacy.aspx`. Fazer com que a página use a página mestra `Site.master` e adicionar a política de privacidade do seu site a esta página. Figura 3 mostra o Visual Studio depois que esta página foi criada.


[![Adicionar uma nova página nomeada &lt;código&gt;Privacy.aspx&lt;/code&gt; para a pasta da raiz do site](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Figura 3**: adicionar uma nova página nomeada `Privacy.aspx` para a pasta da raiz do site ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Em seguida, retorne para a interface do usuário Copy Web Site. Como mostra a Figura 4, o painel esquerdo agora inclui os novos arquivos - `Policy.aspx` e `Policy.aspx.vb`. Além disso, esses arquivos são marcados com um ícone de seta e um Status de novo, indicando que eles existem no site local, mas não no site remoto.


[![A ferramenta Copy Web Site inclui o novo &lt;código&gt;Privacy.aspx&lt;lt;Code>#Date(2011&gt; página em seu painel mais à esquerda](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Figura 4**: A ferramenta Copy Web Site inclui o novo `Privacy.aspx` página no seu painel mais à esquerda ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Para implantar os novos arquivos, selecione-os e, em seguida, clique no ícone de seta para transferi-las para o site remoto. Após a conclusão da transferência de `Policy.aspx` e `Policy.aspx.vb` arquivos existem em ambos os sites locais e remotos com o status Unchanged.

Junto com a listagem de novos arquivos, a ferramenta Copy Web Site destaca todos os arquivos que diferem entre os sites locais e remotos. Para ver isso em ação, retornar o `Privacy.aspx` página e adicione mais algumas palavras para a política de privacidade. Salve a página e, em seguida, retornar para a ferramenta Copy Web Site. Como mostra a Figura 5, o `Privacy.aspx` página no painel à esquerda tem um status de Changed indicando que ele está fora de sincronia com o site remoto.


[![A ferramenta Copy Web Site indica que o &lt;código&gt;Privacy.aspx&lt;lt;Code>#Date(2011&gt; página foi alterada](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Figura 5**: A ferramenta Copy Web Site indica que o `Privacy.aspx` página foi alterada ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image15.png))


A ferramenta Copy Web Site também indica se um arquivo tiver sido excluído desde a última operação de cópia. Excluir o `Privacy.aspx` do projeto local e atualizar a ferramenta Copy Web Site. O `Privacy.aspx` e `Privacy.aspx.vb` arquivos permanecem listados no painel esquerdo, mas têm um status excluído indicando que eles foram removidos desde a última operação de cópia.

## <a name="publishing-a-web-application"></a>Publicar um aplicativo Web

Outra maneira de implantar o aplicativo web no Visual Studio é usar a opção de publicar, que pode ser acessada por meio do menu Build. A opção Publicar explicitamente compila o aplicativo e, em seguida, copia todos os arquivos necessários até o site remoto especificado. Como você verá em breve, a opção Publicar é mais direta do que a ferramenta Copy Web Site. Enquanto a ferramenta Copy Web Site permite que você examine os arquivos nos sites locais e remotos e permite que você carregar ou baixar arquivos individuais conforme necessário, a opção de publicação implanta o aplicativo web inteiro.


Além de copiar todos os arquivos necessários para o site remoto especificado, a opção Publicar explicitamente compila o aplicativo. Considerando que os projetos de aplicativos Web precisam ser explicitamente compilado deve ser nenhuma surpresa que a opção de publicação está disponível para projetos de aplicativos Web. O que pode ser um pouco surpreendente é que a opção de publicação também está disponível para projetos de Site. Conforme observado na [ *determinando quais arquivos precisam ser implantados* ](determining-what-files-need-to-be-deployed-vb.md) tutorial, os projetos de Site pode ser compilado explicitamente por meio de um processo conhecido como *pré-compilação*. Este tutorial concentra-se sobre como usar a opção de publicação com projetos de aplicativos Web; um tutorial futuro explorará pré-compilação, no ponto em que vamos voltar para ver como usar a opção de publicação com projetos de Site.

> [!NOTE]
> Enquanto a opção de publicação está disponível no Visual Studio para projetos de Site e projetos de aplicativos Web, o Visual Web Developer oferece apenas a opção de publicar projetos de aplicativos Web.


Vamos dar uma olhada em como implantar o aplicativo de resenhas de livros usando a opção de publicação. Comece abrindo BookReviewsWAP (no projeto de aplicativo Web) no Visual Studio. No menu Publish, escolha o projeto de Build BookReviewsWAP. Isso abre uma caixa de diálogo que solicita o local de destino, entre outras opções de configuração (veja a Figura 6). Muito semelhante com a ferramenta Copy Web Site você pode inserir um local que aponta para uma pasta local, um site local no IIS, um site remoto que dá suporte a extensões FrontPage Server Extensions, ou um endereço do servidor FTP. Você pode escolher se deseja substituir os arquivos no servidor web remoto com os arquivos implantados ou excluir todo o conteúdo no site remoto antes da publicação. Você também pode especificar se deseja copiar:

- Somente os arquivos no projeto necessários para executar o aplicativo, que omite o código-fonte desnecessários e arquivos relacionados ao projeto.
- Todos os arquivos de projeto, que inclui os arquivos de código-fonte e arquivos de projeto do Visual Studio, como o arquivo de solução.
- Todos os arquivos na pasta de projeto de origem, que copia todos os arquivos na pasta de projeto de origem, independentemente se eles estão incluídos no projeto.

Também há uma opção para carregar o conteúdo do `App_Data` pasta.


[![Especifique o site de destino](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Figura 6**: especifique o site de destino ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Para o aplicativo resenha de livro site remoto contém os arquivos sejam implantados ao copiar o projeto BookReviewsWSP por meio da ferramenta Copy Web Site. Portanto, vamos ter a opção Publicar iniciar excluindo todo o conteúdo existente. Além disso, vamos simplesmente copiar os arquivos necessários em vez de desorganizar o ambiente de produção com arquivos de código e projeto de origem desnecessários. Depois de especificar essas opções, clique no botão Publicar. Sobre os próximos segundos Visual Studio implantará os arquivos necessários para o site de destino, exibindo o progresso na janela de saída.

Figura 7 mostra os arquivos no site de FTP depois que a operação de publicação foi concluída. Observe que apenas as páginas de marcação e os arquivos de suporte necessário sever-lado do cliente e foram carregados.


[![Somente os arquivos necessários foram publicados no ambiente de produção](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Figura 7**: somente o necessário arquivos foram publicadas no ambiente de produção ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image21.png))


A opção de publicação é uma ferramenta menos sutil que a ferramenta Copy Web Site. Enquanto a ferramenta Copy Web Site permite que você inspecione os arquivos nos sites locais e remotos e ver como eles diferem, a opção Publicar não fornece nenhuma interface deste tipo. Além disso, a ferramenta Copy Web Site permite que você faça alterações unitárias, carregar ou excluir arquivos individuais. A opção de publicação não permite esse controle refinado; em vez disso, ele publica os *inteira* aplicativo. Esse comportamento tem seus prós e contras. O lado bom, você pode saber ao usar a opção Publicar que não se esquecer para carregar um arquivo importante. Mas considere o que acontece se você tiver feito uma pequena alteração para um site da Web - muito grande, com a opção de publicação não é possível atualizar essa página, ou duas que foi modificado, mas em vez disso, você deverá aguardar até que o Visual Studio implanta o site inteiro.

Não é incomum para haver certos arquivos cujo conteúdo é diferente entre os ambientes de desenvolvimento e produção. Um exemplo de chave é o arquivo de configuração do aplicativo, `Web.config`. Como a opção Publicar cegamente copia os arquivos do aplicativo web ele substitui os arquivos de configuração personalizada do ambiente de produção com a versão no ambiente de desenvolvimento. O tutorial subsequente explora este tópico ainda mais e oferece dicas para implantar um aplicativo web, quando essas diferenças existem.

## <a name="summary"></a>Resumo

Implantação de um site envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O tutorial anterior mostrou como transferir arquivos usando um cliente FTP como o FileZilla. Este tutorial examinado duas ferramentas de implantação no Visual Studio: a ferramenta Copy Web Site e a opção de publicação. A ferramenta Copy Web Site é semelhante a um cliente de FTP em que ele tem uma interface de dois painéis listando os arquivos no computador local e um computador remoto especificado que torna mais fácil carregar ou baixar arquivos entre os dois computadores. A opção de publicação é uma ferramenta mais insensível explicitamente compila o projeto e, em seguida, implanta o aplicativo inteiro para o destino especificado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Copiar Site da Web com a ferramenta Copy Web Site](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Como eu faço para: implantar um Site usando a ferramenta Copy Web Site](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vídeo)
- [Como: Publicar projetos de aplicativos Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Como: Publicar Sites da Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Instalação e implantação de projetos no Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-an-ftp-client-vb.md)
> [Próximo](common-configuration-differences-between-development-and-production-vb.md)
