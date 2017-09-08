* Startup.cs: [classe inicialização](../fundamentals/startup.md) -classe configura o pipeline de solicitação que manipula todas as solicitações feitas para o aplicativo.
* Program.cs: [classe programa](../fundamentals/index.md) que contém o ponto de entrada principal do aplicativo.
* firstapp.csproj: [arquivo de projeto](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj) formato de arquivo de projeto do MSBuild para aplicativos do ASP.NET Core. Contém referências de projeto a projeto, referências de NuGet e outros itens relacionados do projeto.
* appSettings. JSON / appsettings. Development.JSON: Arquivo de configuração do ambiente base do aplicativo configurações. [Consulte configuração](xref:fundamentals/configuration).
* bower. JSON: Bower as dependências do pacote para o projeto.
* bowerrc: arquivo de configuração bower define onde instalar os componentes quando Bower baixa os ativos.
* bundleconfig.JSON: arquivos de configuração para Empacotando e minimizando ativos front-end do JavaScript e CSS.
* Modos de exibição: Contém os modos de exibição do Razor. Modos de exibição são os componentes que exibem a interface do usuário do aplicativo (IU). Em geral, essa interface do usuário exibe os dados de modelo.
* Controladores: Contém controladores MVC, inicialmente *HomeController*. Os controladores são classes que lidam com solicitações do navegador.
* wwwroot: pasta de raiz do aplicativo Web.

Para obter mais informações, consulte [padrão MVC a](xref:mvc/overview).
