---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Noções básicas sobre a localização do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: A localização é o processo de design e integrar o suporte para um idioma específico e a cultura em um aplicativo ou um componente de aplicativo. O Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="understanding-aspnet-ajax-localization"></a>Noções básicas sobre a localização do ASP.NET AJAX
====================
por [Scott Ndicar](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> A localização é o processo de design e integrar o suporte para um idioma específico e a cultura em um aplicativo ou um componente de aplicativo. A plataforma Microsoft ASP.NET fornece amplo suporte para localização de aplicativos padrão do ASP.NET, integrando o modelo de localização padrão do .NET; o Microsoft AJAX Framework utilizar o modelo integrado para dar suporte os diversos cenários localização em que pode ser executada.


## <a name="introduction"></a>Introdução

Tecnologia ASP.NET da Microsoft oferece um modelo de programação orientada a objeto e controlada por evento e une-lo com os benefícios do código compilado. No entanto, o seu modelo de processamento no lado do servidor tem várias desvantagens inerentes a tecnologia, muitos dos quais podem ser endereçados por novos recursos incluídos no namespace Extensions, que encapsula os serviços da Microsoft AJAX no .NET Framework 3.5. Essas extensões permitem que muitos recursos de cliente avançados disponíveis anteriormente como parte das extensões do AJAX do ASP.NET 2.0, mas agora é parte da biblioteca de classes de Base do Framework. Controles e recursos nesse namespace incluem o processamento parcial de páginas sem exigir uma atualização de página inteira, a capacidade de acessar os serviços da Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma extensa API cliente projetado para espelhar muitas os esquemas de controle vistos no conjunto de controle de servidor ASP.NET.

Este white paper examina os recursos de localização presentes na biblioteca de Script do Microsoft AJAX, do Microsoft AJAX Framework e no contexto da necessidade comercial de suporte à localização e revisando suporte já integrado para a localização na web aplicativos fornecidos pelo .NET Framework. A biblioteca de Script do Microsoft AJAX utiliza o formato. resx arquivo já em uso por aplicativos .NET, que fornece suporte integrado ao IDE e um tipo de recurso podem ser compartilhadas.

Este white paper se baseia na versão Beta 2 do Microsoft Visual Studio 2008. Este white paper também pressupõe que você estará trabalhando com o Visual Studio 2008, não Visual Web Developer Express e fornece instruções passo a passo de acordo com a interface do usuário do Visual Studio. Alguns exemplos de código irão utilizar modelos de projeto que podem não estar disponíveis no Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*A necessidade de localização*

Principalmente para os desenvolvedores de aplicativos corporativos e os desenvolvedores de componentes, a capacidade de criar ferramentas que podem estar cientes das diferenças entre idiomas e culturas se torna cada vez mais necessária. Criando componentes com a capacidade de adaptar-se a localidade do cliente aumenta a produtividade do desenvolvedor e reduz a quantidade de trabalho necessário para a adaptação de um componente para a função global.

A localização é o processo de design e integrar o suporte para um idioma específico e a cultura em um aplicativo ou um componente de aplicativo. A plataforma Microsoft ASP.NET fornece amplo suporte para localização de aplicativos padrão do ASP.NET, integrando o modelo de localização padrão do .NET; o Microsoft AJAX Framework utilizar o modelo integrado para dar suporte os diversos cenários localização em que pode ser executada. Com o Microsoft AJAX Framework, scripts ou podem ser localizados por que está sendo implantado em assemblies de satélite, ou utilizando uma estrutura de sistema de arquivo estático.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Inserção de Scripts com Assemblies satélite*

Consistente com a estratégia de localização padrão do .NET Framework, os recursos podem ser incluídos em assemblies de satélite. Assemblies satélites oferecem diversas vantagens sobre a inclusão de recurso tradicionais em binários - qualquer localização determinada pode ser atualizada sem atualizar a imagem maior, localizações adicionais podem ser implantadas instalando assemblies satélites em a pasta do projeto e assemblies de satélite podem ser implantados sem causar um recarregamento do assembly do projeto principal. Especialmente em projetos do ASP.NET, isso é útil porque ele pode reduzir significativamente a quantidade de recursos do sistema usados por atualizações incrementais, no mínimo interrompe e uso do site de produção.

Scripts são inseridos em assemblies, incluindo os arquivos que estão incluídos no assembly em tempo de compilação. resx gerenciadas (ou compilado. resources). Os recursos são disponibilizados para o aplicativo de script AJAX gerados pelo tempo de execução código, por meio de atributos de nível de assembly

*Convenções de nomenclatura para arquivos de Script inserido*

O gerenciamento de script do Microsoft AJAX Framework oferece suporte a uma variedade de opções para uso na implantação e testes de scripts e diretrizes são fornecidas para facilitar essas opções.

*Para facilitar a depuração:*

Scripts de versão (produção) não devem incluir o `.debug` qualificador no nome de arquivo. Scripts criados para depuração deve incluir `.debug` no nome de arquivo.

*Para facilitar a localização:*

Scripts de cultura neutra não devem incluir qualquer identificador de cultura no nome do arquivo. Para scripts que contêm recursos localizados, o código de idioma ISO deve ser especificado no nome do arquivo. Por exemplo, `es-CO` significa espanhol, Colúmbia Britânica.

A tabela a seguir resume o convenções com exemplos de nomenclatura de arquivo:

| Filename | Significado |
| --- | --- |
| Script.js | Um script de cultura neutra de versão de lançamento. |
| Script.debug.js | Um script de cultura neutra de versão de depuração. |
| Script.en-US.js | Um versão em inglês, Estados Unidos script de liberação. |
| Script.debug.es-CO.js | Um script de Colúmbia Britânica espanhol, versão de depuração. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Passo a passo: Criar um Script inserido, localizado

*Observação: este passo a passo requer o uso do Visual Studio 2008, como Visual Web Developer Express não inclui um modelo de projeto para projetos de biblioteca de classe.*

1. Crie um novo projeto de Site da Web com o ASP.NET AJAX Extensions integrado. Crie outro projeto, um projeto de biblioteca de classes, dentro da solução chamado LocalizingResources.
2. Adicione um arquivo Jscript chamado VerifyDeletion.js ao projeto LocalizingResources, bem como arquivos de recursos. resx chamados DeletionResources.resx e DeletionResources.es.resx. O primeiro contém recursos de cultura neutra; o segundo contém recursos de idioma espanhol.
3. Adicione o seguinte código para VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Para aqueles familiarizados com sintaxe JavaScript Regex, texto dentro de barras único (no exemplo anterior, /FILENAME/ é um exemplo) denota um objeto RegExp. Biblioteca do MSDN contém uma referência abrangente do JavaScript e recursos em objetos de JavaScript nativos podem ser encontradas online.

1. Adicione as seguintes cadeias de caracteres de recurso para DeletionResources.resx: 

    **VerifyDelete**: tem certeza de que deseja excluir o nome de arquivo?

    **Excluído**: nome de arquivo foi excluído.

1. Adicione as seguintes cadeias de caracteres de recurso para DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro que desee quitar FILENAME?

    **Excluído**: nome de arquivo se ha quitado.
2. Adicione as seguintes linhas de código para o arquivo AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Adicione referências a System. Web e Extensions para o projeto LocalizingResources.
2. Adicione uma referência ao projeto LocalizingResources do projeto do Site.
3. Em default.aspx, sob o projeto de Site da Web, atualize o ScriptManager com a seguinte marcação adicional:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Em default.aspx, em qualquer lugar na página, inclua essa marcação:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Pressione F5. Se solicitado, habilite a depuração. Quando a página for carregada, pressione o botão de exclusão. Observe que você será solicitado em inglês (a menos que o computador é definido para preferir recursos de idioma espanhol por padrão) para confirmação.
2. Feche a janela do navegador e retornar para default.aspx. No @Page automático de substituição para Culture e UICulture com es-ES diretiva de cabeçalho. Pressione F5 novamente para iniciar o aplicativo web no navegador novamente. Desta vez, observe que você será solicitado para excluir o arquivo em espanhol:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-localization/_static/image6.png))


Observe que há muitas variações para este passo a passo. Por exemplo, scripts podem ser registrados com o controle ScriptManager programaticamente durante o carregamento de página.

## <a name="including-a-static-script-file-structure"></a>*Incluindo uma estrutura de arquivos de Script estático*

Ao usar arquivos de script estático para a implantação, você perder alguns dos benefícios de usar o esquema de localização .NET inerente. É principalmente visível que você perca o tipo automática gerado a partir de incluindo arquivos de recursos de script; no passo a passo acima, por exemplo, os recursos foram expostos por um tipo gerado automaticamente chamado mensagem do controle ScriptManager.

No entanto, há alguns benefícios de usar uma estrutura de arquivos de script estático. As atualizações podem ser executadas sem recompilar e reimplantar os assemblies satélite e o uso de uma estrutura de arquivo estático também pode ser feito para substituir o script inserido, para integrar uma parte menor de funcionalidades que podem não ter sido enviadas a um componente.

A Microsoft recomenda a evitar um problema de controle de versão ao gerar automaticamente seus recursos de script durante a compilação do projeto. Durante a manutenção de um código de script abrangente base, ele pode se tornar muito difícil garantir que as alterações de código são refletidas em cada script localizados. Como alternativa, você pode simplesmente manter um script de lógica e vários scripts de localização, mesclar os arquivos durante a criação do projeto.

Porque não há recursos a serem incluídos declarativamente, script estático arquivos devem ser referenciadas adicionando `<asp:ScriptElement>` elementos como um filho de `<Scripts>` marca do controle ScriptManager ou adicionando programaticamente `ScriptReference` objetos para o `Scripts` propriedade o `ScriptManager` controle na página em tempo de execução.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*O ScriptManager e sua função na localização*

O ScriptManager permite que vários comportamentos automáticos para aplicativos localizados:

- Ele localiza automaticamente os arquivos de script com base nas definições e convenções de nomenclatura; Por exemplo, ele carrega habilitados para depuração de scripts no modo de depuração, carrega localizadas e scripts com base na seleção de interface do usuário do navegador.
- Isso permite que a definição de culturas, incluindo culturas personalizadas.
- Ele permite a compactação de arquivos de script por meio de HTTP.
- Ele armazena em cache os scripts para gerenciar de maneira eficaz muitas solicitações.
- Ele adiciona uma camada de indireção scripts redirecionando-los por meio de uma URL criptografado.

Referências de script podem ser adicionadas ao controle ScriptManager programaticamente ou por marcação declarativa. Marcação declarativa é particularmente útil ao trabalhar com scripts inseridos em assemblies que não seja o projeto de site da web, como o nome do script não provavelmente será alterado conforme as revisões são enviadas por meio.

## <a name="summary"></a>Resumo

Como aplicativos web crescem para alcançar um público maior, a necessidade de ser capaz de alcançar maior culturas e comunidades fica principal para um modelo de negócios; aplicativos da web de comércio eletrônico precisam ser capaz de lidar com moedas estrangeiras, sistemas de gerenciamento de conteúdo precisam estar presentes e pode não apenas seu conteúdo, mas também suas dicas de navegação e campos de formulário em outros idiomas e as empresas precisam saber que essa necessidade acessível.

O .NET Framework intrinsecamente dá suporte a uma estrutura de localização avançada, utilizando assemblies satélite e arquivos de recursos (. resx) XML para apresentar uma maneira uniforme para pesquisar imagens e cadeias de caracteres de recurso. O ASP.NET AJAX Extensions, incluindo o Microsoft AJAX Framework e a biblioteca de Script do Microsoft AJAX, oferecem suporte para este modelo de programação em código do lado do cliente, habilitando pesquisas de cadeia de caracteres de recurso fácil. Assemblies satélites suportam à inclusão automática de recursos de script (arquivos. js real) por meio de ScriptResource desde que os nomes de arquivo seguem um determinado esquema de nomenclatura. Com esse suporte, o ASP.NET AJAX Extensions simplificar a localização dos scripts e globalização de aplicativos.

## <a name="bio"></a>*Bio*

Scott Cate trabalha com tecnologias Microsoft Web desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especializada em escrever ASP.NET com base em aplicativos voltados para soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado via email em [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Próximo](understanding-asp-net-ajax-web-services.md)
