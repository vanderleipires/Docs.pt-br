---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Implantar o Site usando o Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: O Visual Studio inclui ferramentas para implantar um site. Saiba mais sobre essas ferramentas neste tutorial.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 1c0e094761a92b10555d2cfcef586ad8c2fb8d27
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888871"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Implantar o Site usando o Visual Studio (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> O Visual Studio inclui ferramentas para implantar um site. Saiba mais sobre essas ferramentas neste tutorial.


## <a name="introduction"></a>Introdução

O tutorial anterior analisamos como implantar um aplicativo simples da web ASP.NET em um provedor de host da web. Especificamente, o tutorial mostrado como usar um cliente FTP como FileZilla para transferir os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O Visual Studio também oferece ferramentas internas para facilitar a implantação em um provedor de host da web. Este tutorial examina duas dessas ferramentas: a ferramenta de cópia da Web Site, onde você pode mover os arquivos de e para um servidor web remoto usando o FTP ou extensões de servidor do FrontPage; e a ferramenta de publicação, que copia o site inteiro para um local especificado.


> [!NOTE]
> Outras ferramentas relacionadas à implantação oferecidas pelo Visual Studio incluem [projetos de instalação do Web](https://msdn.microsoft.com/library/wx3b589t.aspx) e [projetos de implantação da Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Projetos de instalação da Web empacotar conteúdo de um site e informações de configuração em um único arquivo MSI. Essa opção é mais útil para sites que são implantados em uma intranet ou para as empresas que vendem um aplicativo web predefinidos que os clientes instalem em seus servidores web. O Add-In de projetos de implantação de Web é que um Visual Studio suplemento que facilita a especificação de diferenças de configuração entre compilações para ambientes de desenvolvimento e ambientes de produção. Projetos de configuração da Web não são abordados nesta série tutorial; Projetos de implantação da Web estão resumidos no [ *diferenças de configuração comuns entre o desenvolvimento e produção* ](common-configuration-differences-between-development-and-production-vb.md) tutorial.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Implantar o Site usando a ferramenta Copiar Site

Ferramenta de cópia da Web Site do Visual Studio é semelhante em funcionalidade a um cliente autônomo do FTP. Em resumo, a ferramenta Copy Web Site permite que você se conectar a um site remoto por meio de FTP ou extensões de servidor do FrontPage. Semelhante à interface do usuário do FileZilla, a interface do usuário copiar Web Site consiste em dois painéis: o painel esquerdo lista os arquivos locais, enquanto o painel direito lista os arquivos no servidor de destino.

> [!NOTE]
> A ferramenta Copiar Site só está disponível para projetos de Site. O Visual Studio oferecem essa ferramenta quando você estiver trabalhando com um projeto de aplicativo Web.


Vamos dar uma olhada usando a ferramenta Copiar Site para publicar o aplicativo de análise de livro para produção. Porque a ferramenta Copiar Site funciona somente com os projetos que usam o modelo de projeto de Site que podemos apenas examinar usando essa ferramenta com o projeto BookReviewsWSP. Abra o projeto.

Inicie o projeto de ferramenta Copy Web Site clicando no ícone de cópia da Web Site no Gerenciador de soluções (esse ícone é circulado na Figura 1); Como alternativa, você pode selecionar a opção cópia de Site no menu do site. Qualquer abordagem inicia a interface de usuário de cópia da Web Site mostrada na Figura 1; apenas o painel esquerdo na Figura 1 é popular porque temos ainda para se conectar a um servidor remoto.


[![Interface do usuário da ferramenta de Site da Web de cópia é dividido em dois painéis](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Figura 1**: Interface a ferramenta Copiar Site do usuário é dividido em dois painéis ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image3.png))


Para implantar o nosso site, é preciso primeiro se conectar ao provedor de host da web. Clique no botão Conectar na parte superior da interface do usuário de Site da Web de cópia. Isso exibe a caixa de diálogo Abrir Site mostrada na Figura 2.

Você pode se conectar ao site de destino, selecionando uma das quatro opções da esquerda:

- **Sistema de arquivos** -Selecione esta opção para implantar seu site para um pasta ou compartilhamento de rede acessível do computador.
- **Local IIS** -use essa opção para implantar o site para o servidor web do IIS instalado no computador.
- **FTP Site** -se conectar a um site remoto usando o FTP.
- **Site remoto** -conectar-se a um site remoto usando as extensões de servidor do FrontPage.

A maioria dos provedores de host web suporte FTP, mas menos oferecem suporte à extensão de servidor do FrontPage. Por esse motivo, eu tiver selecionado a opção de FTP Site e, em seguida, inserir as informações de conexão, conforme mostrado na Figura 2.


[![Especifique o site de destino](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Figura 2**: especifique o site de destino ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Depois de se conectar, a ferramenta Copiar Site carrega os arquivos no site remoto no painel direito e indica o status de cada arquivo: novos, excluídos, alterado ou inalterado. Você pode copiar um arquivo do site local para o site remoto, ou vice-versa, um.

Vamos adicionar uma nova página ao projeto BookReviewsWSP e, em seguida, implantá-lo para que podemos ver a cópia da ferramenta de Site em ação. Criar uma nova página ASP.NET no Visual Studio no diretório raiz chamado `Privacy.aspx`. Tem a página a usar a página mestra `Site.master` e adicionar política de privacidade do seu site a esta página. A Figura 3 mostra o Visual Studio após esta página foi criada.


[![Adicionar uma nova página chamada &lt;código&gt;Privacy.aspx&lt;/código&gt; para a pasta da raiz do site](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Figura 3**: adicionar uma nova página chamada `Privacy.aspx` para a pasta da raiz do site ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Em seguida, retorne para a interface do usuário de Site da Web de cópia. Como mostra a Figura 4, o painel esquerdo agora inclui os novos arquivos - `Policy.aspx` e `Policy.aspx.vb`. Além disso, esses arquivos são marcados com um ícone de seta e um Status de novo, indicando que existam no site local, mas não no site remoto.


[![A ferramenta Copiar Site inclui o novo &lt;código&gt;Privacy.aspx&lt;/código&gt; página no painel esquerdo](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Figura 4**: A ferramenta Copiar Site inclui o novo `Privacy.aspx` página no painel esquerdo ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Para implantar os novos arquivos, selecione-os e, em seguida, clique no ícone de seta para transferi-los para o site remoto. Após a transferência de `Policy.aspx` e `Policy.aspx.vb` arquivos existir em ambos os sites locais e remotos com o status Unchanged.

Junto com a listagem de novos arquivos, a ferramenta Copiar Site destaca todos os arquivos que diferem entre os sites locais e remotos. Para ver isso em ação, retornar o `Privacy.aspx` página e adicionar mais algumas palavras para a política de privacidade. Salve a página e, em seguida, retornar para a cópia da ferramenta de Site. Como mostra a Figura 5, o `Privacy.aspx` página no painel esquerdo tiver um status alterado indicando que ele está fora de sincronia com o site remoto.


[![A ferramenta Copiar Site indica que o &lt;código&gt;Privacy.aspx&lt;/código&gt; página foi alterada](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Figura 5**: A ferramenta Copiar Site indica que o `Privacy.aspx` página foi alterada ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image15.png))


A ferramenta Copy Web Site também indica se um arquivo tiverem sido excluído desde a última operação de cópia. Excluir o `Privacy.aspx` do projeto local e atualizar a cópia da ferramenta de Site. O `Privacy.aspx` e `Privacy.aspx.vb` arquivos permanecem listados no painel esquerdo, mas têm um status excluído indicando que eles foram removidos desde a última operação de cópia.

## <a name="publishing-a-web-application"></a>Publicar um aplicativo Web

Outra maneira de implantar o aplicativo web no Visual Studio é usar a opção de publicação, que pode ser acessada por meio do menu Build. A opção Publicar explicitamente compila o aplicativo e, em seguida, copia todos os arquivos necessários para o site remoto especificado. Como você verá em breve, a opção Publicar é mais direta de cópia da ferramenta de Site. Enquanto a ferramenta Copy Web Site permite que você examine os arquivos nos sites locais e remotos e permite que você carregar ou baixar os arquivos individuais conforme necessário, a opção Publicar implanta o aplicativo web inteiro.


Além de copiar todos os arquivos necessários para o site remoto especificado, a opção Publicar explicitamente compila o aplicativo. Considerando que os projetos de aplicativo Web precisa ser compilado explicitamente deve ser nenhuma surpresa que a opção de publicação está disponível para projetos de aplicativo Web. O que pode ser um pouco surpreendente é que a opção Publicar também está disponível para projetos de Site. Conforme observado no [ *determinar quais arquivos precisam ser implantados* ](determining-what-files-need-to-be-deployed-vb.md) tutorial, projetos de Site pode ser compilado explicitamente por meio de um processo conhecido como *pré-compilação*. Este tutorial concentra-se sobre como usar a opção Publicar com projetos de aplicativo da Web; um tutorial futuras explorará pré-compilação, no ponto em que retornaremos para examinar usando a opção de publicação com projetos de Site.

> [!NOTE]
> Enquanto a opção de publicação está disponível no Visual Studio para projetos de Site e projetos de aplicativo Web, o Visual Web Developer oferece apenas a opção de publicar projetos de aplicativos Web.


Vamos dar uma olhada em implantar o aplicativo de revisões de livros usando a opção de publicação. Comece abrindo BookReviewsWAP (o projeto de aplicativo Web) no Visual Studio. No menu de publicação, escolha o projeto de compilação BookReviewsWAP. Isso abre uma caixa de diálogo que solicita o local de destino, entre outras opções de configuração (consulte a Figura 6). Muito semelhante com a ferramenta Copy Web Site você pode inserir um local que aponta para uma pasta local, um site da Web no IIS local, um site remoto que ofereça suporte a extensões ou um endereço do servidor FTP. Você pode escolher se deseja substituir os arquivos no servidor web remoto com os arquivos de implantação ou excluir todo o conteúdo no site remoto antes da publicação. Você também pode especificar se deseja copiar:

- Somente os arquivos no projeto necessários para executar o aplicativo, que omite o código-fonte desnecessários e arquivos relacionados ao projeto.
- Todos os arquivos de projeto, que inclui os arquivos de código fonte e arquivos de projeto do Visual Studio, como o arquivo de solução.
- Todos os arquivos na pasta de projeto de origem, que copia todos os arquivos na pasta de projeto de origem, independentemente se eles estão incluídos no projeto.

Há também uma opção para carregar o conteúdo de `App_Data` pasta.


[![Especifique o site de destino](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Figura 6**: especifique o site de destino ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Para o aplicativo de análise de livro o site remoto contém os arquivos implantados ao copiar o projeto BookReviewsWSP por meio de cópia da ferramenta de Site. Portanto, vamos tem a opção de publicar iniciar excluindo todo o conteúdo existente. Além disso, vamos apenas copiar os arquivos necessários em vez de encher o ambiente de produção com arquivos de projeto e código fonte desnecessários. Depois de especificar essas opções, clique no botão Publicar. Sobre os próximos segundos Visual Studio implantará os arquivos necessários para o site de destino, exibir seu progresso na janela de saída.

A Figura 7 mostra os arquivos no site FTP após a operação de publicação. Observe que apenas as páginas de marcação e os arquivos de suporte necessários de servidor e cliente-lado foi carregados.


[![Somente os arquivos necessários foram publicados no ambiente de produção](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Figura 7**: apenas o necessário arquivos foram publicados no ambiente de produção ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-visual-studio-vb/_static/image21.png))


A opção de publicação é uma ferramenta menos sutis que a cópia da ferramenta de Site. Enquanto a ferramenta Copy Web Site permite que você inspecione os arquivos nos sites locais e remotos e ver como elas podem diferir, a opção Publicar fornece essa interface não. Além disso, a ferramenta Copy Web Site permite que você faça alterações únicos, carregar ou excluir arquivos individuais. A opção de publicação não permite esse controle refinado; em vez disso, ela publica o *todo* aplicativo. Esse comportamento tem seus prós e contras. No lado do sinal de adição, você pode saber ao usar a opção Publicar que não esquecer que você carregue um arquivo importante. Porém, considere o que acontece se você fez uma pequena alteração para um site grande - com a opção de publicação não é possível atualizar essa página ou dois que tenha sido modificado, mas em vez disso, você deverá aguardar até que o Visual Studio implanta a todo o site.

Não é incomum para haver certos arquivos cujo conteúdo é diferente entre os ambientes de desenvolvimento e produção. Um exemplo de chave é o arquivo de configuração do aplicativo, `Web.config`. Como a opção Publicar cegamente copia os arquivos do aplicativo web ele substitui os arquivos de configuração personalizada do ambiente de produção com a versão no ambiente de desenvolvimento. O tutorial subsequente explora mais neste tópico e oferece dicas para implantar um aplicativo web quando essas diferenças.

## <a name="summary"></a>Resumo

Implantar um site envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O tutorial anterior mostrou como transferir arquivos usando um cliente FTP como FileZilla. Este tutorial examinadas duas ferramentas de implantação no Visual Studio: a cópia da ferramenta de Site e a opção de publicação. A ferramenta Copy Web Site é semelhante a um cliente FTP em que ele tem uma interface de dois painéis listando os arquivos no computador local e um computador remoto especificado que torna mais fácil carregar ou baixar os arquivos entre os dois computadores. A opção de publicação é uma ferramenta mais moderada explicitamente compila o projeto e, em seguida, implanta o aplicativo inteiro para o destino especificado.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Copiar Site com a ferramenta Copy Web Site](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Como os i: implantar um Site usando a ferramenta Copiar Site](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vídeo)
- [Como: Publicar projetos de aplicativo Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Como: Publicar Sites da Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Instalação e implantação de projetos no Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-an-ftp-client-vb.md)
> [Próximo](common-configuration-differences-between-development-and-production-vb.md)
