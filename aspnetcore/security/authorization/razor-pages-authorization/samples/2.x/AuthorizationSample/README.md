# <a name="aspnet-core-authorization-sample"></a>Exemplo de autorização de ASP.NET Core

Este exemplo ilustra o uso de autorização de páginas Razor pelas convenções. Este exemplo demonstra os recursos descritos a [convenções de autorização de páginas Razor](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) tópico.

Autorização de usuário neste exemplo usa a autenticação de cookie recursos descritos a [usar autenticação de cookie sem a identidade do ASP.NET Core](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) tópico. Para obter informações sobre como usar a identidade do ASP.NET Core, consulte os tópicos de [autenticação](https://docs.microsoft.com/aspnet/core/security/authentication/index) seção da documentação.

Ao executar o exemplo, use o endereço de email **maria.rodriguez@contoso.com** para autenticar o usuário.

## <a name="examples-in-this-sample"></a>Exemplos desta amostra

| Recurso | Descrição |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Adiciona um [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para a página com o caminho especificado. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Adiciona um [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) para todas as páginas em uma pasta com o caminho especificado. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Adiciona um [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para uma página com o caminho especificado. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Adiciona um [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) para todas as páginas em uma pasta com o caminho especificado. |
