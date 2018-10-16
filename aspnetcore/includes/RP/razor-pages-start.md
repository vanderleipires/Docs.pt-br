O modelo padrão cria os links e páginas **RazorPagesMovie**, **Início**, **Sobre** e **Contato**. Dependendo do tamanho da janela do navegador, talvez você precise clicar no ícone de navegação para mostrar os links.

![Página Inicial ou de Índice](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Teste os links. Os links **RazorPagesMovie** e **Início** vão para a página de Índice. Os links **Sobre** e **Contato** vão para as páginas `About` e `Contact`, respectivamente.

## <a name="project-files-and-folders"></a>Pastas e arquivos de projeto

A tabela a seguir lista os arquivos e pastas no projeto. Para este tutorial, o arquivo *Startup.cs* é o mais importante de se entender. Não é necessário examinar cada link fornecido abaixo. Os links são fornecidos como uma referência para quando você precisar de mais informações sobre um arquivo ou pasta no projeto.

| Arquivo ou pasta              | Finalidade |
| ----------------- | ------------ |
| wwwroot | Contém arquivos estáticos. Confira [Arquivos estáticos](xref:fundamentals/static-files). |
| Páginas | Pasta para [Páginas do Razor](xref:razor-pages/index). |
| *appsettings.json* | [Configuração](xref:fundamentals/configuration/index) |
| *Program.cs* | [Hospeda](xref:fundamentals/host/index) o aplicativo ASP.NET Core.|
| *Startup.cs* | Configura os serviços e o pipeline de solicitação. Consulte [Inicialização](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>A pasta Páginas

O arquivo *_Layout.cshtml* contém elementos HTML comuns (scripts e folhas de estilo) e define o layout para o aplicativo. Por exemplo, quando você clica em **RazorPagesMovie**, **Início**, **Sobre** ou **Contato**, você vê os mesmos elementos. Os elementos comuns incluem o menu de navegação na parte superior e o cabeçalho na parte inferior da janela. Veja [Layout](xref:mvc/views/layout) para obter mais informações.

O arquivo *_ViewImports.cshtml* contém diretivas do Razor que são importadas para cada Página do Razor. Veja [Importando diretivas compartilhadas](xref:mvc/views/layout#importing-shared-directives) para obter mais informações.

O *_ViewStart.cshtml* define a propriedade `Layout` das Páginas do Razor para usar o arquivo *_Layout.cshtml*. Veja [Layout](xref:mvc/views/layout) para obter mais informações.

O arquivo *_ValidationScriptsPartial.cshtml* fornece uma referência a scripts de validação [jQuery](https://jquery.com/). Quando adicionarmos as páginas `Create` e `Edit` posteriormente no tutorial, o arquivo *_ValidationScriptsPartial.cshtml* será usado.

As páginas `About`, `Contact` e `Index` são páginas básicas que você pode usar para iniciar um aplicativo. A página `Error` é usada para exibir informações de erro. A página `Privacy` permite especificar detalhes sobre a política de privacidade do site.
