---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Noções básicas sobre a localização do AJAX ASP.NET | Microsoft Docs
author: scottcate
description: Localização é o processo de criação e a integração do suporte para um idioma ou cultura específico em um aplicativo ou um componente de aplicativo. O Mic...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 86cbf150708f1db711b40ccbc25345afeb3e542a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825170"
---
<a name="understanding-aspnet-ajax-localization"></a>Noções básicas sobre a localização do AJAX ASP.NET
====================
por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Localização é o processo de criação e a integração do suporte para um idioma ou cultura específico em um aplicativo ou um componente de aplicativo. A plataforma Microsoft ASP.NET fornece amplo suporte para a localização para aplicativos do ASP.NET padrão, integrando o modelo de localização do .NET standard; a estrutura do Microsoft AJAX utilizam o modelo integrado para dar suporte os diversos cenários em que localização pode ser executada.


## <a name="introduction"></a>Introdução

Tecnologia ASP.NET da Microsoft oferece um modelo de programação orientada a objeto e controlada por evento e une-lo com os benefícios do código compilado. No entanto, seu modelo de processamento do lado do servidor tem várias desvantagens inerentes da tecnologia, muitos dos quais podem ser tratados, os novos recursos incluídos no namespace Extensions, que encapsula os serviços AJAX da Microsoft no .NET Framework 3.5. Essas extensões permitem muitos recursos sofisticados, anteriormente disponíveis como parte das extensões do ASP.NET 2.0 AJAX, mas agora parte da biblioteca de classes Base do Framework. Controles e recursos nesse namespace incluem o processamento parcial de páginas sem exigir uma atualização de página inteira, a capacidade de acessar serviços Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma extensa API do lado do cliente é projetado para espelhar a muitos dos os esquemas de controle vistos no conjunto de controle de servidor ASP.NET.

Este white paper examina os recursos de localização presentes na estrutura do Microsoft AJAX e biblioteca de Script do Microsoft AJAX, no contexto de negócios que precisa de suporte à localização e revisando suporte já integrado para localização na web aplicativos fornecidos pelo .NET Framework. A biblioteca de scripts do Microsoft AJAX utiliza o formato do arquivo já usado por aplicativos .NET, que fornece suporte integrado a IDE e um tipo de recurso podem ser compartilhadas.

Este white paper se baseia na versão Beta 2 do Microsoft Visual Studio 2008. Este white paper também pressupõe que você estará trabalhando com o Visual Studio 2008, não Visual Web Developer Express e fornecerá instruções passo a passo de acordo com a interface do usuário do Visual Studio. Alguns exemplos de código irá utilizar modelos de projeto que podem não estar disponíveis no Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*A necessidade de localização*

Especialmente para os desenvolvedores de aplicativos empresariais e os desenvolvedores de componentes, a capacidade de criar ferramentas que podem estar cientes das diferenças entre idiomas e culturas se tornou cada vez mais necessária. Criar componentes com a capacidade de adaptar-se à localidade do cliente aumenta a produtividade do desenvolvedor e reduz a quantidade de trabalho necessária para a adaptação de um componente funcione globalmente.

Localização é o processo de criação e a integração do suporte para um idioma ou cultura específico em um aplicativo ou um componente de aplicativo. A plataforma Microsoft ASP.NET fornece amplo suporte para a localização para aplicativos do ASP.NET padrão, integrando o modelo de localização do .NET standard; a estrutura do Microsoft AJAX utilizam o modelo integrado para dar suporte os diversos cenários em que localização pode ser executada. Com a estrutura do Microsoft AJAX, scripts ou podem ser localizados por que está sendo implantado em assemblies satélite, ou utilizando uma estrutura de sistema de arquivos estáticos.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Inclusão de Scripts com Assemblies de satélite*

Consistente com a estratégia de localização padrão do .NET Framework, os recursos podem ser incluídos em assemblies satélites. Assemblies satélites oferecem diversas vantagens sobre a inclusão de recurso tradicionais em binários - qualquer determinada localização pode ser atualizada sem atualizar a imagem ampliada, suas localizações adicionais podem ser implantadas instalando assemblies satélites em a pasta do projeto e assemblies de satélite podem ser implantados sem causar um recarregamento do assembly do projeto principal. Particularmente em projetos do ASP.NET, isso é benéfico porque ele pode reduzir significativamente a quantidade de recursos do sistema usados por atualizações incrementais e minimamente interrompe o uso do site de produção.

Scripts são inseridos em assemblies, incluindo-os em gerenciados. resx (ou compilada. resources) arquivos que estão incluídos no assembly em tempo de compilação. Seus recursos são então disponibilizados para o aplicativo de script por meio de AJAX gerados em tempo de execução pelo código, por meio de atributos de nível de assembly

*Convenções de nomenclatura para arquivos de Script inserido*

O gerenciamento de script do Microsoft AJAX Framework dá suporte a uma variedade de opções para uso na implantação e testar scripts e as diretrizes são fornecidas para facilitar essas opções.

*Para facilitar a depuração:*

Scripts de versão (produção) não devem incluir o `.debug` qualificador no nome do arquivo. Scripts projetados para depuração deve incluir `.debug` no nome do arquivo.

*Para facilitar a localização:*

Scripts de cultura neutra não devem incluir qualquer identificador de cultura no nome do arquivo. Para scripts que contêm recursos localizados, o código de idioma ISO deve ser especificado no nome do arquivo. Por exemplo, `es-CO` significa espanhol, Colúmbia.

A tabela a seguir resume o convenções com exemplos de nomenclatura de arquivo:

| Filename | Significado |
| --- | --- |
| Script. js | Um script de cultura neutra de versão de lançamento. |
| Script.debug.js | Um script de cultura neutra de versão de depuração. |
| Script.en US.js | Um versão em inglês, Estados Unidos script de liberação. |
| Script.debug.es-CO.js | Um script de Columbia espanhol, versão de depuração. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Passo a passo: Criar um Script inserido, localizado

*Observação: este passo a passo requer o uso do Visual Studio 2008, como o Visual Web Developer Express não inclui um modelo de projeto para projetos de biblioteca de classe.*

1. Crie um novo projeto de Site da Web com extensões AJAX do ASP.NET integrado. Crie outro projeto, um projeto de biblioteca de classes, dentro da solução chamado LocalizingResources.
2. Adicione um arquivo Jscript chamado VerifyDeletion.js ao projeto LocalizingResources, bem como arquivos de recursos. resx chamados DeletionResources.resx e DeletionResources.es.resx. O primeiro contém recursos de cultura neutra; o segundo conterá os recursos de idioma espanhol.
3. Adicione o seguinte código ao VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Para aqueles familiarizados com a sintaxe JavaScript Regex, o texto dentro do único barras "/" (no exemplo anterior, /FILENAME/ é um exemplo) denota um objeto RegExp. A biblioteca MSDN contém uma referência abrangente do JavaScript e recursos em objetos nativos do JavaScript podem ser encontrados online.

1. Adicione as seguintes cadeias de caracteres de recurso para DeletionResources.resx: 

    **VerifyDelete**: tem certeza de que deseja excluir o nome de arquivo?

    **Excluído**: nome de arquivo foi excluído.

1. Adicione as seguintes cadeias de caracteres de recurso para DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro que desee quitar nome de arquivo?

    **Excluído**: nome de arquivo se ha quitado.
2. Adicione as seguintes linhas de código ao arquivo AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Adicione referências a System. Web e Extensions para o projeto LocalizingResources.
2. Adicione uma referência ao projeto LocalizingResources do projeto do Site.
3. Em Default. aspx, sob o projeto de Site da Web, atualize o controle ScriptManager com a seguinte marcação adicional:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Em Default. aspx, em qualquer lugar na página, inclua essa marcação:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Pressione F5. Se solicitado, habilite a depuração. Quando a página for carregada, pressione o botão Excluir. Observe que você será solicitado em inglês (a menos que seu computador é configurado para dar preferência a recursos de idioma espanhol por padrão) para confirmação.
2. Feche a janela do navegador e retorne para default. aspx. No @Page diretiva de cabeçalho, automática de substituição para Culture e UICulture com es-ES. Pressione F5 novamente para iniciar o aplicativo web no navegador novamente. Desta vez, observe que você será solicitado para excluir o arquivo em espanhol:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-localization/_static/image6.png))


Observe que há diversas variações para este passo a passo. Por exemplo, scripts pôde ser registrados com o controle ScriptManager programaticamente durante o carregamento da página.

## <a name="including-a-static-script-file-structure"></a>*Incluindo uma estrutura de arquivos de Script estático*

Ao usar arquivos de script estático para a implantação, você perde alguns dos benefícios de usar o esquema de localização .NET inerente. É principalmente visível que você perca o tipo automático gerado a partir de incluindo arquivos de recurso de script; no passo a passo acima, por exemplo, os recursos foram expostos por um tipo gerado automaticamente, chamado de mensagem do controle do ScriptManager.

No entanto, há alguns benefícios em usar uma estrutura de arquivos de script estático. As atualizações podem ser executadas sem recompilar e reimplantar os assemblies de satélite, e o uso de uma estrutura de arquivo estático também pode ser feito para substituir o script inserido, para integrar uma parte menor de funcionalidades que podem não ter sido enviadas com um componente.

A Microsoft recomenda evitando um problema de controle de versão ao gerar automaticamente seus recursos de script durante a compilação do projeto. Durante a manutenção de um código de script extensivo base, ele pode se tornar cada vez mais difícil garantir que as alterações de código são refletidas em cada script localizados. Como alternativa, você pode simplesmente manter um script de lógica e vários scripts de localização, mesclar os arquivos durante a criação do projeto.

Porque não há recursos a serem incluídos declarativamente, os arquivos devem ser estáticos de script referenciado adicionando `<asp:ScriptElement>` elementos como um filho do `<Scripts>` marca do controle ScriptManager, ou adicionando programaticamente `ScriptReference` objetos para o `Scripts` propriedade do `ScriptManager` controle na página em tempo de execução.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*O ScriptManager e sua função na localização*

O ScriptManager habilita vários comportamentos automáticos para aplicativos localizados:

- Ele localiza automaticamente os arquivos de script com base nas configurações e as convenções de nomenclatura; Por exemplo, ele carrega os scripts de depuração habilitados quando no modo de depuração e cargas localizadas scripts com base na seleção de interface do usuário do navegador.
- Ele permite a definição das culturas, incluindo culturas personalizadas.
- Ele permite que a compactação de arquivos de script por meio de HTTP.
- Ele armazena em cache os scripts para gerenciar com eficiência de muitas solicitações.
- Ele adiciona uma camada de indireção para scripts Canalizando-los por meio de uma URL criptografado.

Referências de script podem ser adicionadas ao controle ScriptManager programaticamente ou por marcação declarativa. Marcação declarativa é particularmente útil ao trabalhar com scripts inseridos em assemblies que não seja o projeto de site em si, como o nome do script não provavelmente será alterado conforme as revisões são enviados por push por meio do.

## <a name="summary"></a>Resumo

À medida que crescem de aplicativos web para alcançar um público maior, a necessidade de ser capaz de alcançar mais amplo de culturas e comunidades torna-se essencial para um modelo de negócios; aplicativos da web de comércio eletrônico precisam ser capaz de lidar com moedas estrangeiras, sistemas de gerenciamento de conteúdo precisam estar presente capaz não apenas seu conteúdo, mas também suas dicas de navegação e campos de formulário em outros idiomas e as empresas precisam saber que essa necessidade é acessível.

O .NET Framework intrinsecamente dá suporte a uma estrutura avançada de localização, utilizando os assemblies de satélite e arquivos de recurso (. resx) de XML para apresentar uma maneira uniforme para procurar imagens e cadeias de caracteres de recurso. Extensões AJAX do ASP.NET, incluindo a estrutura do Microsoft AJAX e a biblioteca de Script do Microsoft AJAX, oferecem suporte para esse modelo de programação em código do lado do cliente, permitindo pesquisas de cadeia de caracteres de recursos fácil. Assemblies satélites suporte à inclusão automática de recursos de script (arquivos. js real) por meio de ScriptResource desde que os nomes de arquivo seguem um determinado esquema de nomenclatura. Com esse suporte, as extensões do AJAX ASP.NET simplificar a localização dos scripts e a globalização de aplicativos.

## <a name="bio"></a>*Biografia*

Scott Cate tem trabalhado com tecnologias Web Microsoft desde 1997 e é o presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especialista na escrita de ASP.NET com base em aplicativos com foco em soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado através do email [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Próximo](understanding-asp-net-ajax-web-services.md)
