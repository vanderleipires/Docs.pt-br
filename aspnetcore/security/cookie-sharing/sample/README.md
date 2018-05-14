# <a name="cookie-sharing-sample-app"></a>Aplicativo de exemplo de compartilhamento de cookie

O exemplo ilustra o cookie compartilhamento entre três aplicativos que usam autenticação de cookie:

| Projeto                             | Descrição |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | Aplicativo do ASP.NET Core 2.0 Razor páginas sem usar a identidade do ASP.NET Core |
| CookieAuthWithIdentity.Core         | Aplicativo do ASP.NET Core 2.0 MVC com a identidade do ASP.NET Core |
| CookieAuthWithIdentity.NETFramework | Aplicativo do MVC do ASP.NET Framework 4.6.1 com a identidade do ASP.NET |

Instruções:

1. Execute o aplicativo CookieAuth.Core. Registre um usuário. O aplicativo autentica o usuário quando o usuário está registrado. Desconecte o usuário.
1. Na mesma sessão do navegador, execute o aplicativo CookieAuthWithIdentity.Core. Registre o mesmo usuário conforme usado com o aplicativo principal. O aplicativo autentica o usuário quando o usuário está registrado. Desconecte o usuário.
1. Na mesma sessão do navegador, execute o aplicativo CookieAuthWithIdentity.NETFramework. Registre o mesmo usuário conforme usado com outros aplicativos. O aplicativo autentica o usuário quando o usuário está registrado. Desconecte o usuário.
1. Faça logon do usuário para qualquer um dos três aplicativos. O cookie de autenticação é compartilhado entre os aplicativos. Observe que o usuário é conectado automaticamente ao dois aplicativos.
1. Desconecte o usuário de qualquer um dos aplicativos. Observe que o usuário é automaticamente desconectado de dois aplicativos.

Este exemplo demonstra os recursos descritos a [compartilhar cookies entre aplicativos](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tópico.
