---
uid: mvc/overview/performance/bundling-and-minification
title: Agrupamento e Minificação | Microsoft Docs
author: Rick-Anderson
description: Agrupamento e minificação são duas técnicas você pode usar no ASP.NET 4.5 para melhorar o tempo de carregamento da solicitação. Agrupamento e minificação melhora o tempo de carregamento, reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 7e54bdd2f50edb5001982ada9b6ce023584ce5b0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823497"
---
<a name="bundling-and-minification"></a>Agrupamento e Minificação
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Agrupamento e minificação são duas técnicas você pode usar no ASP.NET 4.5 para melhorar o tempo de carregamento da solicitação. Agrupamento e minificação melhora o tempo de carregamento, reduzindo o número de solicitações para o servidor e reduzindo o tamanho dos ativos mais solicitados (como CSS e JavaScript.)


A maioria dos principais navegadores atuais limita o número de [conexões simultâneas](http://www.browserscope.org/?category=network) por cada nome de host para seis. Isso significa que, enquanto estão sendo processadas seis solicitações, solicitações adicionais para ativos em um host serão enfileiradas pelo navegador. Na imagem abaixo, as guias de rede de ferramentas de desenvolvedor F12 do IE mostra o tempo para os ativos necessários para a exibição sobre de um aplicativo de exemplo.

![B/M](bundling-and-minification/_static/image1.png)

As barras em cinza mostram o tempo que a solicitação está na fila pelo navegador aguardando o limite de seis conexão. A barra amarela é o tempo da solicitação até o primeiro byte, ou seja, o tempo necessário para enviar a solicitação e receber a primeira resposta do servidor. As barras azuis mostram o tempo necessário para receber os dados de resposta do servidor. Você pode clicar duas vezes em um ativo para obter informações detalhadas de tempo. Por exemplo, a imagem a seguir mostra os detalhes de medição de tempo de carregamento a */Scripts/MyScripts/JavaScript6.js* arquivo.

![](bundling-and-minification/_static/image2.png)

Mostra a imagem anterior a **iniciar** evento, que dá a hora em que a solicitação foi enfileirada devido ao navegador limita o número de conexões simultâneas. Nesse caso, a solicitação foi enfileirada para 46 milissegundos aguarda outra solicitação concluir.

## <a name="bundling"></a>Agrupamento

O agrupamento é um novo recurso no ASP.NET 4.5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. Você pode criar outros pacotes, JavaScript e CSS. Menos arquivos significa menos solicitações HTTP e que pode melhorar o desempenho de carregamento de página primeiro.

A imagem a seguir mostra a mesma exibição de medição de tempo do modo de exibição sobre mostrada anteriormente, mas desta vez com o agrupamento e minificação habilitado.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimização

Minimização executa uma variedade de otimizações de código diferente para scripts ou css, como remover espaços em branco desnecessários e comentários e encurtar os nomes de variável para um caractere. Considere a seguinte função de JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Depois de minimização, a função é reduzida para o seguinte:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Além de remover os comentários e espaço em branco desnecessário, os nomes de variáveis e parâmetros a seguir foram renomeados (reduzido) da seguinte maneira:

| **Original** | **Renomeado** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impacto de agrupamento e Minificação

A tabela a seguir mostra várias diferenças importantes entre listar todos os ativos individualmente e usar o agrupamento e minificação (B/M) no programa de exemplo.

|  | **Usando B/M** | **Sem B/M** | **Alteração** |
| --- | --- | --- | --- |
| **Solicitações de arquivos** | 9 | 34 | 256% |
| **KB enviada** | 3.26 | 11.92 | 266% |
| **KB recebido** | 388.51 | 530 | 36% |
| **O tempo de carregamento** | 510 MS | 780 MS | 53% |

Os bytes enviados tinham uma redução significativa com agrupamento como navegadores são bastante detalhados com os cabeçalhos HTTP que eles se aplicam a solicitações. A redução de bytes recebidos não é tão grande porque os maiores arquivos (*Scripts\\jquery-ui-1.8.11.min.js* e *Scripts\\1.7.1.min.js jquery*) já são reduzidos . Observação: Os intervalos em que o programa de exemplo usado o [Fiddler](http://www.fiddler2.com/fiddler2/) ferramenta para simular uma rede lenta. (Do Fiddler **regras** menu, selecione **desempenho** , em seguida, **simular velocidades de Modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Depuração agrupado e Minificado JavaScript

É fácil de depurar o JavaScript em um ambiente de desenvolvimento (em que o [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) na *Web. config* arquivo é definido como `debug="true"` ) porque os arquivos JavaScript não são agrupados ou reduzidos. Você também pode depurar um build de versão em que os arquivos JavaScript são agrupados e minificados. Usando as ferramentas de desenvolvedor F12 do IE, você depurar uma função JavaScript incluída em um pacote minimizado usando a seguinte abordagem:

1. Selecione o **Script** e, em seguida, selecione o **iniciar depuração** botão.
2. Selecione o pacote que contém a função de JavaScript que você deseja depurar usando o botão de ativos.  
    ![](bundling-and-minification/_static/image4.png)
3. Formate o JavaScript reduzido, selecionando o **botão de configuração** ![](bundling-and-minification/_static/image5.png)e, em seguida, selecionando **formato JavaScript**.
4. No **Script de pesquisa** caixa de entrada, selecione o nome da função que você deseja depurar. Na imagem a seguir **AddAltToImg** digitado na **Script de pesquisa** caixa de entrada.  
    ![](bundling-and-minification/_static/image6.png)

Para obter mais informações sobre a depuração com as ferramentas de desenvolvedor F12, consulte o artigo do MSDN [usando as ferramentas de desenvolvedor F12 para depurar erros de JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controle de agrupamento e Minificação

Empacotamento e minimização está habilitado ou desabilitado, definindo o valor do atributo de depuração na [Prvek compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) na *Web. config* arquivo. No XML a seguir, `debug` é definido como true, isso o agrupamento e minificação está desabilitada.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Para habilitar o agrupamento e minificação, defina o `debug` valor como "false". Você pode substituir a *Web. config* com o `EnableOptimizations` propriedade o `BundleTable` classe. O código a seguir permite que o agrupamento e minificação e substitui qualquer configuração na *Web. config* arquivo.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A menos que `EnableOptimizations` é `true` ou o atributo de depuração na [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no *Web. config* arquivo é definido como `false`, arquivos não serão agrupados ou reduzidos. Além disso, a versão .min dos arquivos não será usada, as versões de depuração completa serão selecionadas. `EnableOptimizations` substitui o atributo de depuração na [Prvek compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) na *Web. config* arquivo


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Usando o agrupamento e Minificação com Web Forms do ASP.NET e páginas da Web

- Para páginas da Web, consulte a entrada de blog [adicionando a otimização da Web a um Site de páginas da Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Para formulários da Web, consulte a entrada de blog [adicionando agrupamento e Minificação para os Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Usando o agrupamento e Minificação ASP.NET MVC

Nesta seção vamos criar um ASP.NET MVC de projeto para examinar o agrupamento e minificação. Primeiro, crie um novo projeto de internet do ASP.NET MVC denominado **MvcBM** sem alterar qualquer um dos padrões.

Abra o *App\\\_iniciar\\BundleConfig.cs* de arquivo e examine o `RegisterBundles` método que é usado para criar, registrar e configurar pacotes. O código a seguir mostra uma parte do `RegisterBundles` método.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

O código anterior cria um novo pacote de JavaScript chamado *~/bundles/jquery* que inclui todos os apropriado (que é a depuração ou reduzidos, mas não. *VSDoc*) arquivos em de *Scripts* pasta que correspondem à cadeia de caractere curinga "~/Scripts/jquery-{version}. js". Para o ASP.NET MVC 4, isso significa que com uma configuração de depuração, o arquivo *jquery 1.7.1.js* será adicionado ao pacote. Em uma configuração de versão *jquery 1.7.1.min.js* será adicionado. A estrutura de empacotamento segue várias convenções comuns, como:

- Selecionar arquivo de ".min" para a versão quando *FileX.min.js* e *FileX.js* existe.
- Selecionando a versão de não ".min" para depuração.
- Ignorando "-vsdoc" arquivos (como *jquery-1.7.1-VSDoc*), que são usados apenas pelo IntelliSense.

O `{version}` mostrado acima de correspondência de curinga é usado para criar automaticamente um pacote jQuery com a versão apropriada do jQuery em seu *Scripts* pasta. Neste exemplo, usar um caractere curinga oferece os seguintes benefícios:

- Permite que você use o NuGet para atualizar para uma versão mais recente de jQuery sem alterar o código anterior de agrupamento ou referências de jQuery em suas páginas de exibição.
- Seleciona a versão completa para as configurações de depuração e a versão de ".min" versão compila automaticamente.

## <a name="using-a-cdn"></a>Usar uma CDN

 O código a seguir substitui o pacote jQuery local com um pacote de jQuery da CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

No código acima, o jQuery será solicitado da CDN enquanto versão modo e a versão de depuração do jQuery serão obtidos localmente no modo de depuração. Ao usar uma CDN, você deve ter um mecanismo de fallback em caso de falha de solicitação CDN. A seguinte marcação de fragmento do final do script layout arquivo mostra adicionado à solicitação de jQuery se a falhar CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Criando um pacote

O [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `Include` método pega uma matriz de cadeias de caracteres, onde cada cadeia de caracteres é um caminho virtual para o recurso. O código a seguir da `RegisterBundles` método na *App\\\_iniciar\\BundleConfig.cs* arquivo mostra como vários arquivos forem adicionados a um pacote:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

O [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` método é fornecido para adicionar todos os arquivos em um diretório (e, opcionalmente, todos os subdiretórios) que correspondem a um padrão de pesquisa. O [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` API é mostrada abaixo:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pacotes são referenciados nos modos de exibição usando o método de renderização, (`Styles.Render` para o CSS e `Scripts.Render` para JavaScript). A seguinte marcação do *modos de exibição\\Shared\\\_layout. cshtml* arquivo mostra como os modos de exibição do projeto de internet do ASP.NET padrão fazer referência a pacotes CSS e JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Observe que os métodos de renderização pega uma matriz de cadeias de caracteres, portanto, você pode adicionar vários pacotes em uma linha de código. Em geral, você desejará usar os métodos de renderização que cria o HTML necessário para referenciar o ativo. Você pode usar o `Url` método para gerar a URL para o ativo sem a marcação necessária para referenciar o ativo. Suponha que você gostaria de usar o novo HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atributo. O código a seguir mostra como referenciar o modernizr usando o `Url` método.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Usando o "\*" caractere curinga para selecionar arquivos

O caminho virtual especificado na `Include` método e a pesquisa padrão em de `IncludeDirectory` método pode aceitar um "\*" caractere curinga como um prefixo ou sufixo no último segmento de caminho. A cadeia de caracteres de pesquisa diferencia maiusculas de minúsculas. O `IncludeDirectory` método tem a opção de pesquisa subdiretórios.

Considere um projeto com os arquivos JavaScript a seguir:

- *Scripts\\comuns\\AddAltToImg.js*
- *Scripts\\comuns\\ToggleDiv.js*
- *Scripts\\comuns\\ToggleImg.js*
- *Scripts\\comuns\\Sub1\\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

A tabela a seguir mostra os arquivos adicionados a um pacote usando o caractere curinga, conforme mostrado:

| **Call** | **Arquivos adicionados ou a exceção gerada** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Exceção padrão inválido. O caractere curinga só é permitido no prefixo ou sufixo. |
| Include("~/Scripts/Common/\*og.\*") | Exceção padrão inválido. Somente um caractere curinga é permitido. |
| Incluir ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Incluir ("~/Scripts/Common/\*") | Exceção padrão inválido. Um segmento curinga puro não é válido. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Adicionar explicitamente cada arquivo para um pacote é geralmente a preferida ao longo de carregamento de curinga de arquivos pelos seguintes motivos:

- Adicionando scripts, os padrões de curinga para carregá-los em ordem alfabética, que normalmente não é o que você quer. Arquivos CSS e JavaScript frequentemente precisam ser adicionados em uma ordem específica de (não alfabéticos). Você pode reduzir esse risco, adicionando um personalizado [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementação, mas adicionar explicitamente cada arquivo está menos sujeita a erros. Por exemplo, você pode adicionar novos ativos para uma pasta no futuro, que podem exigir que você modifique sua [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementação.
- Arquivos específicos do modo de exibição adicionados a um diretório usando o caractere curinga ao carregar podem ser incluídos em todas as exibições referenciando esse pacote. Se o script de modo de exibição específico é adicionado a um pacote, você poderá receber um erro de JavaScript em outras exibições que referenciam o pacote.
- Os arquivos CSS que importar outros arquivos resultam em arquivos importados carregados duas vezes. Por exemplo, o código a seguir cria um pacote com a maioria dos arquivos CSS de jQuery UI tema carregados duas vezes. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  O seletor de curinga "\*. css" traz em cada arquivo CSS na pasta, incluindo o *conteúdo\\temas\\base\\jquery.ui.all.css* arquivo. O *jquery.ui.all.css* arquivo importa os outros arquivos CSS.

## <a name="bundle-caching"></a>Cache de pacote

Pacotes de definir cabeçalho HTTP expira um ano de quando o pacote é criado. Se você navegar até uma página exibida anteriormente, mostra Fiddler IE não faz uma solicitação condicional para o pacote, ou seja, há nenhuma solicitação HTTP GET do IE para os pacotes e nenhuma resposta HTTP 304 do servidor. Você pode forçar o IE para fazer uma solicitação condicional para cada pacote com a tecla F5 (resultando em uma resposta HTTP 304 para cada pacote). Você pode forçar uma atualização completa usando ^ F5 (resultando em uma resposta HTTP 200 para cada pacote).

A imagem a seguir mostra a **Caching** guia do painel de resposta do Fiddler:

![imagem de cache do Fiddler](bundling-and-minification/_static/image8.png)

A solicitação   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 é para o pacote **AllMyScripts** e contém um par de cadeia de caracteres de consulta **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. A cadeia de caracteres de consulta **v** tem um valor de token que é um identificador exclusivo usado para armazenar em cache. Desde que o pacote não é alterado, o aplicativo ASP.NET solicitará a **AllMyScripts** usando este token de pacote. Se qualquer arquivo do pacote for alterado, a estrutura de otimização do ASP.NET irá gerar um novo token, garantindo que as solicitações do navegador para o pacote receberá o pacote mais recente.

Se você executar as ferramentas de desenvolvedor F12 do IE9 e navega até uma página carregada anteriormente, IE incorretamente mostra as solicitações GET condicionais feitas para cada pacote e o servidor retornar HTTP 304. Você pode ler o motivo pelo qual IE9 apresenta problemas para determinar se houve uma solicitação condicional na entrada de blog [CDNs usando e Expires para melhorar o desempenho do Site da Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MENOR, CoffeeScript, SCSS, Sass agrupamento.

A estrutura de agrupamento e minificação fornece um mecanismo para processar idiomas intermediários, como [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [menos](http://www.dotlesscss.org/) ou [Coffeescript ](http://coffeescript.org/)e aplicar transformações, como minificação para o pacote resultante. Por exemplo, para adicionar [empacotará](http://www.dotlesscss.org/) arquivos ao seu projeto do MVC 4:

1. Crie uma pasta para o seu conteúdo menor. O exemplo a seguir usa o *conteúdo\\MyLess* pasta.
2. Adicione a [empacotará](http://www.dotlesscss.org/) pacote do NuGet **sem pingo** ao seu projeto.  
    ![Instalação do NuGet sem pingo](bundling-and-minification/_static/image9.png)
3. Adicionar uma classe que implementa o [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interface. Para a transformação empacotará, adicione o seguinte código ao seu projeto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Criar um pacote de arquivos LESS com o `LessTransform` e o [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformar. Adicione o seguinte código para o `RegisterBundles` método na *App\\iniciar\\BundleConfig.cs* arquivo.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Adicione o seguinte código para todas as exibições que referencia o pacote de menos.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considerações de pacote

É uma boa convenção a seguir ao criar pacotes incluir o "pacote" como um prefixo no nome do pacote. Isso impedirá que um possível [conflito de roteamento](https://forums.asp.net/post/5012037.aspx).

Depois que você atualiza um arquivo em um pacote, um novo token é gerado para o parâmetro de cadeia de caracteres de consulta de pacote e o pacote completo deve ser baixado na próxima vez que um cliente solicita uma página que contém o pacote. Na marcação tradicional onde cada ativo é listado individualmente, somente o arquivo alterado seria baixado. Ativos que mudam com frequência podem não ser bons candidatos para agrupamento.

Agrupamento e minificação basicamente melhoram o tempo de carregamento de solicitação de página primeiro. Depois que uma página da Web foi solicitada, o navegador armazena em cache os ativos (JavaScript, CSS e imagens) para que o agrupamento e minificação não fornecerá qualquer aumento no desempenho ao solicitar a mesma página ou páginas no mesmo site solicitando os mesmos recursos. Se você não definir a expira cabeçalho corretamente em seus ativos e você não use agrupamento e minificação, a heurística de atualização dos navegadores marcará os ativos obsoletos depois de alguns dias e o navegador exigirá uma solicitação de validação para cada ativo. Nesse caso, o agrupamento e minificação fornecem um aumento de desempenho após a primeira solicitação de página. Para obter detalhes, consulte o blog [CDNs usando e Expires para melhorar o desempenho do Site da Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

A limitação do navegador de seis conexões simultâneas por cada nome de host pode ser reduzida usando um [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Como a CDN terá um nome de host diferente que seu site de hospedagem, solicitações de ativos da CDN não afetarão o limite de conexões simultâneas de seis para seu ambiente de hospedagem. Uma CDN também pode fornecer o cache de pacotes comuns e vantagens de cache de borda.

Pacotes devem ser particionados por páginas que precisam delas. Por exemplo, o modelo do ASP.NET MVC para um aplicativo de internet padrão cria um conjunto de validação do jQuery separado do jQuery. Como modos de exibição padrão criados não tem nenhuma entrada e não valores de postagem, elas não incluem o pacote de validação.

O `System.Web.Optimization` namespace é implementado no *System.Web.Optimization.dll*. Ele aproveita a biblioteca WebGrease (*WebGrease.dll*) para recursos de minimização, que por sua vez usa *Antlr3.Runtime.dll*.

*Usar o Twitter para fazer postagens rápidas e compartilhar links. Meu identificador do Twitter é*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Recursos adicionais

- Vídeo:[empacotamento e otimizando](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) por [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Adicionando a otimização da Web a um Site de páginas da Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Adicionar agrupamento e Minificação para os Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicações de desempenho de agrupamento e Minificação na navegação na Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) por [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Usando as CDNs e expira para melhorar o desempenho do Site da Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) de Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimizar o RTT (tempos de ida e volta)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Colaboradores

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
