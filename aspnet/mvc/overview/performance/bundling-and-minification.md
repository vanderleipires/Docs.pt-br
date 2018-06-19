---
uid: mvc/overview/performance/bundling-and-minification
title: Empacotamento e minimização | Microsoft Docs
author: Rick-Anderson
description: Empacotamento e minimização são duas técnicas você pode usar no ASP.NET 4.5 para melhorar o tempo de carga de solicitação. Empacotamento e minimização melhora o tempo de carregamento por reducin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
ms.locfileid: "30877477"
---
<a name="bundling-and-minification"></a>Empacotamento e minimização
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Empacotamento e minimização são duas técnicas você pode usar no ASP.NET 4.5 para melhorar o tempo de carga de solicitação. Empacotamento e minimização melhora o tempo de carregamento, reduzindo o número de solicitações para o servidor e reduzindo o tamanho dos ativos solicitados (por exemplo, CSS e JavaScript)


A maioria dos navegadores principais atuais limita o número de [conexões simultâneas](http://www.browserscope.org/?category=network) por cada nome de host para seis. Isso significa que enquanto estão sendo processadas seis solicitações, solicitações adicionais de ativos em um host serão enfileiradas pelo navegador. Na imagem abaixo, as guias de rede de ferramentas de desenvolvedor F12 do IE mostra o tempo para os ativos necessários para o modo de exibição sobre um aplicativo de exemplo.

![M/B](bundling-and-minification/_static/image1.png)

As barras em cinza mostram a hora em que a solicitação estiver enfileirada pelo navegador aguardando o limite de seis conexão. A barra amarela é o tempo da solicitação até o primeiro byte, ou seja, o tempo necessário para enviar a solicitação e receber a primeira resposta do servidor. As barras azuis mostram o tempo necessário para receber os dados de resposta do servidor. Você pode clicar duas vezes em um ativo para obter informações detalhadas de tempo. Por exemplo, a imagem a seguir mostra os detalhes de tempo para carregar o */Scripts/MyScripts/JavaScript6.js* arquivo.

![](bundling-and-minification/_static/image2.png)

Mostra a imagem anterior a **iniciar** evento, que fornece a hora em que a solicitação foi enfileirada devido ao navegador limita o número de conexões simultâneas. Nesse caso, a solicitação foi enfileirada para 46 milissegundos aguardando a conclusão da outra solicitação.

## <a name="bundling"></a>Agrupamento

Agrupamento é um novo recurso no ASP.NET 4.5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo. Você pode criar outros pacotes, JavaScript e CSS. Menos arquivos significa menos solicitações HTTP e que pode melhorar o desempenho de carregamento de página primeiro.

A imagem a seguir mostra a mesma exibição de tempo do modo de exibição sobre mostrada anteriormente, mas desta vez com o empacotamento e minimização habilitado.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimização

Minimização executa várias otimizações de código diferente para scripts ou css, como remover comentários e espaços em branco desnecessários e reduzindo os nomes de variável para um caractere. Considere a seguinte função de JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Depois de minimização, a função será reduzida para o seguinte:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Além de remover os comentários e espaços em branco desnecessários, os nomes de variáveis e parâmetros a seguir foram renomeados (reduzido) da seguinte maneira:

| **Original** | **Renomeado** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impacto de empacotamento e minimização

A tabela a seguir mostra várias diferenças importantes entre listando todos os ativos individualmente e usar o empacotamento e minimização (B/M) no programa de exemplo.

|  | **Usando o M/B** | **Sem B/M** | **Change** |
| --- | --- | --- | --- |
| **Solicitações de arquivo** | 9 | 34 | 256% |
| **KB enviada** | 3.26 | 11.92 | 266% |
| **KB recebido** | 388.51 | 530 | 36% |
| **Tempo de carregamento** | 510 MS | 780 MS | 53% |

Os bytes enviados tinham uma redução significativa com agrupamento como navegadores são bastante detalhados com os cabeçalhos HTTP que se aplicam em solicitações. A redução de bytes recebido não é tão grande porque os arquivos maiores (*Scripts\jquery-ui-1.8.11.min.js* e *Scripts\jquery-1.7.1.min.js*) já está minimizada. Observação: Os intervalos em que o programa de exemplo usado o [Fiddler](http://www.fiddler2.com/fiddler2/) ferramenta para simular uma rede lenta. (Do Fiddler **regras** menu, selecione **desempenho** , em seguida, **simular velocidades de Modem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Pacote de depuração e minimizada JavaScript

É fácil depurar o JavaScript em um ambiente de desenvolvimento (onde o [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no *Web. config* arquivo está definido como `debug="true"` ) porque os arquivos JavaScript não são incluídos ou minimizada. Você também pode depurar um build de versão em que os arquivos JavaScript são agrupados e minimizados. Usando as ferramentas de desenvolvedor F12 do IE, depurar uma função JavaScript incluída em um pacote minimizado usando a abordagem a seguir:

1. Selecione o **Script** guia e, em seguida, selecione o **iniciar a depuração** botão.
2. Selecione o pacote que contém a função JavaScript que você deseja depurar usando o botão de ativos.  
    ![](bundling-and-minification/_static/image4.png)
3. Formate o JavaScript minimizado, selecionando o **botão configuração** ![](bundling-and-minification/_static/image5.png)e, em seguida, selecionando **formato JavaScript**.
4. No **script pesquisa** caixa de entrada de t, selecione o nome da função que você deseja depurar. Na imagem a seguir, **AddAltToImg** foi inserido no **script pesquisa** caixa de entrada de t.  
    ![](bundling-and-minification/_static/image6.png)

Para obter mais informações sobre como depurar com as ferramentas de desenvolvedor F12, consulte o artigo do MSDN [usando as ferramentas de desenvolvedor F12 para depurar erros de JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controle de empacotamento e minimização

Empacotamento e minimização está habilitado ou desabilitado, definindo o valor do atributo em depuração o [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no *Web. config* arquivo. No XML a seguir, `debug` é definido como true isso empacotamento e minimização está desabilitada.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Para habilitar o empacotamento e minimização, defina o `debug` valor como "false". Você pode substituir o *Web. config* configuração com o `EnableOptimizations` propriedade o `BundleTable` classe. O código a seguir habilita o empacotamento e minimização e substitui qualquer configuração no *Web. config* arquivo.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A menos que `EnableOptimizations` é `true` ou o atributo de depuração no [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no *Web. config* arquivo é definido como `false`, arquivos não serão agrupados ou minimizados. Além disso, a versão .min dos arquivos não será usada, as versões de depuração completa serão selecionadas. `EnableOptimizations` substitui o atributo de depuração no [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) no *Web. config* arquivo


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Usando o empacotamento e minimização com Web Forms do ASP.NET e páginas da Web

- Para páginas da Web, consulte a postagem do blog [adicionando otimização da Web a um Site de páginas da Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Para formulários da Web, consulte a postagem do blog [adicionando empacotamento e minimização de Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Usando o empacotamento e minimização com o ASP.NET MVC

Esta seção criaremos um ASP.NET MVC projeto para examinar o empacotamento e minimização. Primeiro, crie um novo projeto ASP.NET MVC internet denominado **MvcBM** sem alterar qualquer um dos padrões.

Abra o *aplicativo\_Start\BundleConfig.cs* de arquivo e examine o `RegisterBundles` método que é usado para criar, registrar e configurar pacotes. O código a seguir mostra uma parte do `RegisterBundles` método.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

O código anterior cria um novo pacote de JavaScript chamado *~/bundles/jquery* que inclui todos os apropriado (que é a depuração ou minimizada mas não. *VSDoc*) arquivos de *Scripts* pasta que corresponde a cadeia de caracteres curinga "~/Scripts/jquery-{versão}. js". Para o ASP.NET MVC 4, isso significa que com uma configuração de depuração, o arquivo *1.7.1.js jquery* será adicionado ao pacote. Em uma configuração de versão, *1.7.1.min.js jquery* será adicionado. A estrutura de empacotamento segue várias convenções comuns como:

- Selecionando arquivo versão ".min" quando "FileX.min.js" e "FileX.js" existem.
- Selecione a versão de não ".min" para depuração.
- Ignorando "-vsdoc" arquivos (como jquery-1.7.1-vsdoc.js), que são usados apenas pelo IntelliSense.

O `{version}` mostrado acima de correspondência de curinga é usado para criar automaticamente um pacote jQuery com a versão apropriada do jQuery em seu *Scripts* pasta. Neste exemplo, usar um curinga fornece os seguintes benefícios:

- Permite que você use NuGet para atualizar para uma versão mais recente do jQuery sem alterar o código de empacotamento anterior ou jQuery referências nas suas páginas de exibição.
- Seleciona a versão completa de configurações de depuração e a versão de ".min" para versão cria automaticamente.

## <a name="using-a-cdn"></a>Usando uma CDN

 O código a seguir substitui o pacote jQuery local com um pacote de jQuery CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

No código acima, jQuery será solicitada da CDN enquanto versão modo e a versão de depuração do jQuery serão obtidos localmente no modo de depuração. Ao usar um CDN, você deve ter um mecanismo de fallback no caso de falha de solicitação de CDN. A seguinte marcação de fragmento do final do script layout arquivo mostra adicionado à solicitação jQuery deve falhar a CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Criando um pacote

O [pacote](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `Include` método pega uma matriz de cadeias de caracteres, onde cada cadeia de caracteres é um caminho virtual para o recurso. O código a seguir do método no RegisterBundles o *aplicativo\_Start\BundleConfig.cs* arquivo mostra como vários arquivos são adicionados a um pacote:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

O [pacote](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` método é fornecido para adicionar todos os arquivos em um diretório (e, opcionalmente, todos os subdiretórios) que correspondem a um padrão de pesquisa. O [pacote](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) classe `IncludeDirectory` API é mostrada abaixo:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pacotes que são referenciados nos modos de exibição usando o método de renderização ( `Styles.Render` de CSS e `Scripts.Render` para JavaScript). A seguinte marcação do *exibições \ compartilhadas\\cshtml* arquivo mostra como as exibições de projeto padrão ASP.NET internet fazer referência a pacotes de CSS e JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Observe que os métodos de renderização pega uma matriz de cadeias de caracteres, assim você pode adicionar vários pacotes em uma linha de código. Geralmente, você desejará usar os métodos de renderização que cria o HTML necessário para referenciar o ativo. Você pode usar o `Url` método para gerar a URL para o ativo sem a marcação necessária para referenciar o ativo. Suponha que você deseja usar o novo HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atributo. O código a seguir mostra como referenciar modernizr usando o `Url` método.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Usando o "\*" caractere curinga para selecionar arquivos

O caminho virtual especificado no `Include` método e a pesquisa de padrões no `IncludeDirectory` método pode aceitar um "\*" caractere curinga como um prefixo ou sufixo no último segmento de caminho. A cadeia de caracteres de pesquisa diferencia maiusculas de minúsculas. O `IncludeDirectory` método tem a opção de pesquisa subdiretórios.

Considere um projeto com os seguintes arquivos JavaScript:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![dir imag](bundling-and-minification/_static/image7.png)

A tabela a seguir mostra os arquivos adicionados a um pacote usando o caractere curinga, conforme mostrado:

| **Call** | **Arquivos adicionados ou exceção** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Exceção padrão inválido. O caractere curinga é permitido apenas no prefixo ou sufixo. |
| Include("~/Scripts/Common/\*og.\*") | Exceção padrão inválido. Somente um caractere curinga é permitido. |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | Exceção padrão inválido. Um segmento de curinga puro não é válido. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Adicionar explicitamente cada arquivo para um pacote é geralmente a preferência sobre carregamento de curinga de arquivos pelos seguintes motivos:

- Adicionando scripts por padrões de curinga para carregá-los em ordem alfabética, que é normalmente não o que você deseja. Arquivos CSS e JavaScript frequentemente precisam ser adicionados em uma ordem (não alfabéticos) específica. Você pode reduzir esse risco, adicionando um personalizado [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementação, mas adicionar explicitamente cada arquivo é menos propenso a erros. Por exemplo, você pode adicionar novos ativos para uma pasta no futuro, que podem exigir a modificação de seu [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementação.
- Exibir arquivos específicos adicionados a um diretório usando o curinga de carregamento podem ser incluídos em todas as exibições que referenciam o pacote. Se o script de modo de exibição específico é adicionado a um pacote, você pode receber um erro de JavaScript em outras exibições que referenciam o pacote.
- Arquivos CSS que importar outros arquivos resultam em arquivos importados carregados duas vezes. Por exemplo, o código a seguir cria um pacote com a maioria dos arquivos CSS do jQuery UI tema carregados duas vezes. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  O seletor de curinga "\*. css" coloca em cada arquivo CSS na pasta, incluindo o *Content\themes\base\jquery.ui.all.css* arquivo. O *jquery.ui.all.css* arquivo importa outros arquivos CSS.

## <a name="bundle-caching"></a>Pacote de cache

Pacotes de definir o cabeçalho HTTP expira um ano de quando o pacote é criado. Se você navegar até uma página anteriormente exibida, mostra Fiddler IE não faz uma solicitação condicional para o pacote, ou seja, há nenhuma solicitação HTTP GET do IE para os pacotes e nenhuma resposta 304 HTTP do servidor. Você pode forçar o IE para fazer uma solicitação condicional para cada pacote com a tecla F5 (resultando em uma resposta 304 HTTP para cada pacote). Você pode forçar uma atualização completa usando ^ F5 (resultando em uma resposta HTTP 200 para cada pacote).

A imagem a seguir mostra o **cache** guia do painel de resposta Fiddler:

![imagem de cache do Fiddler](bundling-and-minification/_static/image8.png)

A solicitação   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 para o pacote de **AllMyScripts** e contém um par de cadeia de caracteres de consulta **v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. A cadeia de caracteres de consulta **v** tem um valor de token que é um identificador exclusivo usado para armazenar em cache. Desde que o pacote não são alterados, o aplicativo ASP.NET solicitará o **AllMyScripts** agrupar usando este token. Se qualquer arquivo no conjunto de alterações, a estrutura de otimização do ASP.NET irá gerar um novo token, garantindo que as solicitações de navegador para o pacote obterá o pacote mais recente.

Se você executar as ferramentas de desenvolvedor F12 IE9 e navegue até uma página previamente carregada, IE incorretamente mostra as solicitações GET condicionais feitas para cada pacote e o servidor retornando HTTP 304. Você pode ler o motivo pelo qual IE9 apresenta problemas para determinar se houve uma solicitação condicional na entrada de blog [usando CDNs e expira para melhorar o desempenho do Site](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MENOR, CoffeeScript, SCSS, Sass agrupamento.

A estrutura de empacotamento e minimização fornece um mecanismo para processar idiomas intermediários, como [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [menos](http://www.dotlesscss.org/) ou [Coffeescript ](http://coffeescript.org/)e aplicar transformações como minimização para o conjunto resultante. Por exemplo, para adicionar [.less](http://www.dotlesscss.org/) arquivos ao projeto MVC 4:

1. Crie uma pasta para o seu conteúdo menor. O exemplo a seguir usa o *Content\MyLess* pasta.
2. Adicionar o [.less](http://www.dotlesscss.org/) pacote NuGet **sem ponto** ao seu projeto.  
    ![Instalar sem ponto NuGet](bundling-and-minification/_static/image9.png)
3. Adicionar uma classe que implementa o [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interface. Para a transformação .less, adicione o seguinte código ao seu projeto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Criar um pacote de menos arquivos com o `LessTransform` e [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformação. Adicione o seguinte código para o `RegisterBundles` método o *aplicativo\_Start\BundleConfig.cs* arquivo.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Adicione o seguinte código a qualquer exibição que referencia o pacote menor.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Considerações de pacote

É uma boa convenção a seguir ao criar pacotes de incluir o "pacote" como um prefixo no nome do pacote. Isso impedirá um possível [conflito de roteamento de](https://forums.asp.net/post/5012037.aspx).

Quando você atualiza um arquivo em um pacote, um novo token é gerado para o parâmetro de cadeia de caracteres de consulta de pacote e o pacote completo deve ser baixado na próxima vez que um cliente solicita uma página que contém o pacote. Na marcação tradicional onde cada ativo é listado individualmente, somente o arquivo alterado seria baixado. Ativos que são alterados com frequência não podem ser bons candidatos para agrupamento.

Empacotamento e minimização principalmente melhoram o tempo de carregamento de solicitação de página primeiro. Depois que uma página da Web foi solicitada, o navegador armazena em cache os ativos (JavaScript, CSS e imagens) para o empacotamento e minimização não fornecerá qualquer aumento de desempenho ao solicitar a mesma página ou páginas no mesmo site solicitando os mesmo ativos. Se você não definir o expira cabeçalho corretamente em seus ativos e você não usar o empacotamento e minimização, a heurística de atualização de navegadores marcará os ativos obsoletos depois de alguns dias e o navegador exigirá uma solicitação de validação para cada ativo. Nesse caso, empacotamento e minimização fornecem um aumento de desempenho após a primeira solicitação de página. Para obter detalhes, consulte o blog [usando CDNs e expira para melhorar o desempenho do Site](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

A limitação do navegador de seis conexões simultâneas por cada nome de host pode ser reduzida usando um [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Como o CDN terá um nome de host diferente que seu site de hospedagem, pedidos da CDN não afetará o limite de conexões simultâneas seis para seu ambiente de hospedagem. Um CDN também pode fornecer o cache comum de pacote e vantagens de cache de borda.

Pacotes devem ser particionados por páginas que precisam delas. Por exemplo, o modelo do ASP.NET MVC para um aplicativo de internet padrão cria um pacote de validação jQuery separado do jQuery. Como as exibições padrão criadas não têm nenhuma entrada e não publique os valores, eles não incluem o pacote de validação.

O `System.Web.Optimization` namespace é implementado em System.Web.Optimization.DLL. Ele utiliza a biblioteca de WebGrease (WebGrease.dll) para recursos de minimização, que por sua vez, usa Antlr3.Runtime.dll.

*Uso Twitter tornar postagens rápidas e compartilhar links. É o identificador do Twitter*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Recursos adicionais

- Vídeo:[empacotamento e otimizando](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) por [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Adicionando a otimização da Web a um Site de páginas da Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [A adição de empacotamento e minimização de formulários da Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicações de desempenho de empacotamento e minimização na navegação na Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) por [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Usando CDNs e expirar para melhorar o desempenho do Site da Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) de Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimizar o tempo de resposta (horários de ida e volta)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Colaboradores

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
