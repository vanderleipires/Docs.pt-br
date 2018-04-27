---
title: Gerenciar pacotes do lado do cliente com Bower no ASP.NET Core
author: rick-anderson
description: Gerenciar pacotes do lado do cliente com Bower.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: ada8120189baf036296b83f91d20b364ee90d074
ms.sourcegitcommit: 07903a1be39a99dcf538d57981161592d0e658b8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/20/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Gerenciar pacotes do lado do cliente com Bower no ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), e [Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> Enquanto Bower é mantida, seus mantenedores recomendam usando uma solução diferente. [O Gerenciador de biblioteca](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan abreviada) é o novo sistema de gerenciamento de conteúdo estático do lado do cliente do Visual Studio. Yarn com Webpack é uma alternativa popular para a qual [instruções de migração](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) estão disponíveis.

[Bower](https://bower.io/) chama a mesmo "Um Gerenciador de pacotes para a web". No ecossistema do .NET, ele preenche essa lacuna deixado pela incapacidade do NuGet para entregar os arquivos de conteúdo estáticos. Para projetos do ASP.NET Core, esses arquivos estáticos são inerentes a bibliotecas de cliente como [jQuery](http://jquery.com/) e [inicialização](http://getbootstrap.com/). Para bibliotecas .NET, você usar [NuGet](https://www.nuget.org/) Gerenciador de pacotes.

Processo de compilação de projetos novos criados com os modelos de projeto do ASP.NET Core configurar lado do cliente. [jQuery](http://jquery.com/) e [Bootstrap](http://getbootstrap.com/) são instalados, e tem suporte em Bower.

Pacotes do lado do cliente são listados no *bower. JSON* arquivo. Configura os modelos de projeto do ASP.NET Core *bower. JSON* com inicialização, validação jQuery e jQuery.

Neste tutorial, vamos adicionar suporte para [fonte Awesome](http://fontawesome.io). Bower pacotes podem ser instalados com o **gerenciar pacotes de Bower** interface do usuário ou manualmente o *bower. JSON* arquivo.

### <a name="installation-via-manage-bower-packages-ui"></a>Instalação por meio de gerenciar Bower pacotes da interface do usuário

* Criar um novo aplicativo Web do ASP.NET Core com o **aplicativo Web do ASP.NET Core (.NET Core)** modelo. Selecione **aplicativo Web** e **nenhuma autenticação**.

* Clique com botão direito no projeto no Gerenciador de soluções e selecione **gerenciar pacotes de Bower** (como alternativa no menu principal, **projeto** > **gerenciar pacotes Bower**).

* No **Bower: \<nome do projeto\>**  janela, clique na guia de "Procurar" e, em seguida, filtre a lista de pacotes inserindo `font-awesome` na caixa de pesquisa:

  ![Gerenciar pacotes bower](bower/_static/manage-bower-packages.png)

* Confirme se o "Salvar alterações em *bower. JSON*" caixa de seleção é marcada. Selecione uma versão na lista suspensa e clique no **instalar** botão. O **saída** janela mostra os detalhes da instalação.

### <a name="manual-installation-in-bowerjson"></a>Instalação manual em bower. JSON

Abra o *bower. JSON* e adicione "fonte o incríveis" para as dependências. IntelliSense mostra os pacotes disponíveis. Quando um pacote é selecionado, as versões disponíveis são exibidas. As imagens abaixo são mais antigas e não coincidir com o que você vê.

![IntelliSense do Explorador de pacotes bower](bower/_static/add-package.png)

![versão bower IntelliSense](bower/_static/version-intelliSense.png)

Bower usa [controle de versão semântico](http://semver.org/) para organizar as dependências. Controle de versão semântico, também conhecido como SemVer, identifica os pacotes com o esquema de numeração \<principal >.\< secundária >. \<patch >. IntelliSense simplifica o controle de versão semântico, mostrando apenas algumas opções comuns. O item superior na lista do IntelliSense (4.6.3 no exemplo acima) é considerado a versão estável mais recente do pacote. O símbolo de acento circunflexo (^) corresponde a mais recente versão principal e o til (~) corresponde a versão secundária mais recente.

Salve o *bower. JSON* arquivo. O Visual Studio inspeciona o *bower. JSON* arquivo para que as alterações. Ao salvar, o *bower instalar* comando é executado. Consulte a janela de saída **npm/Bower** modo de exibição para o comando executado.

Abra o *bowerrc* de arquivos em *bower. JSON*. O `directory` está definida como *wwwroot/lib* que indica o local Bower instalará os ativos de pacote.

```json
{
 "directory": "wwwroot/lib"
}
```

Você pode usar a caixa de pesquisa no Gerenciador de soluções para localizar e exibir o pacote incrível de fonte.

Abra o *exibições \ compartilhadas\_cshtml* de arquivo e adicione o arquivo CSS incrível de fonte para o ambiente [auxiliar de marca](xref:mvc/views/tag-helpers/intro) para `Development`. No Gerenciador de soluções, arrastar e soltar *fonte awesome.css* dentro de `<environment names="Development">` elemento.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

Um aplicativo de produção, você adicionaria *fonte awesome.min.css* para o auxiliar de marca de ambiente para `Staging,Production`.

Substitua o conteúdo do *Views\Home\About.cshtml* arquivo Razor com a seguinte marcação:

[!code-html[](bower/sample/About.cshtml)]

Execute o aplicativo e navegue até o modo de exibição sobre para verificar o funcionamento do pacote incrível de fonte.

## <a name="exploring-the-client-side-build-process"></a>Explorando o processo de compilação do lado do cliente

A maioria dos modelos de projeto do ASP.NET Core já estão configurados para usar o Bower. Este passo a passo próxima começa com um projeto vazio do ASP.NET Core e adiciona cada pedaço manualmente, portanto você pode ter uma ideia de como Bower é usado em um projeto. Você pode ver o que acontece com a estrutura do projeto e o tempo de execução de saída como cada alteração de configuração é feita.

As etapas gerais para usar o processo de compilação do lado do cliente com Bower são:

* Defina pacotes usados em seu projeto. <!-- once defined, you don't need to download them, VS does -->
* Pacotes de referência de suas páginas da web.

### <a name="define-packages"></a>Definir pacotes

Depois que você listar pacotes no *bower. JSON* arquivo, o Visual Studio irá baixá-los. O exemplo a seguir usa Bower para carregar jQuery e inicialização para o *wwwroot* pasta.

* Criar um novo aplicativo Web do ASP.NET Core com o **aplicativo Web do ASP.NET Core (.NET Core)** modelo. Selecione o **vazio** modelo de projeto e clique em **Okey**.

* No Gerenciador de soluções, clique com botão direito no projeto > **Adicionar Novo Item** e selecione **Bower arquivo de configuração**. Observação: A *bowerrc* arquivo também é adicionado.

* Abra *bower. JSON*, adicione jquery e inicialização para o `dependencies` seção. Resultante *bower. JSON* arquivo se parecerá com o exemplo a seguir. As versões serão alterado ao longo do tempo e podem não corresponder a imagem a seguir.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Salve o *bower. JSON* arquivo.

  Verifique se o projeto inclui o *bootstrap* e *jQuery* diretórios *wwwroot/lib*. Bower usa o *bowerrc* arquivo para instalar os ativos em *wwwroot/lib*.

  Observação: A interface do usuário "Gerenciar pacotes de Bower" fornece uma alternativa à edição de arquivos manual.

### <a name="enable-static-files"></a>Habilitar arquivos estáticos

* Adicionar o `Microsoft.AspNetCore.StaticFiles` pacote NuGet para o projeto.
* Habilitar arquivos estáticos sejam atendidos com o [middleware de arquivo estático](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions). Adicionar uma chamada para [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) para o `Configure` método `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Pacotes de referência

Nesta seção, você criará uma página HTML para verificar se que ele pode acessar os pacotes implantados.

* Adicionar uma nova página HTML chamada *Index.html* para o *wwwroot* pasta. Observação: Você deve adicionar o arquivo HTML para o *wwwroot* pasta. Por padrão, o conteúdo estático não pode ser servido fora *wwwroot*. Consulte [trabalhar com arquivos estáticos](xref:fundamentals/static-files) para obter mais informações.

  Substitua o conteúdo do *index* com a seguinte marcação:

[!code-html[](bower/sample/Index.html)]

* Execute o aplicativo e navegue até `http://localhost:<port>/Index.html`. Como alternativa, com *Index.html* aberta, pressione `Ctrl+Shift+W`. Verifique se que o estilo de jumbotron é aplicado, o código jQuery responde quando o botão é clicado e que o botão inicialização muda de estado.

  ![estilo de jumbotron aplicado](bower/_static/jumbotron.png)
