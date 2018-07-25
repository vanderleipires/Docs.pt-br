# <a name="cookie-sharing-sample-app"></a>Aplicativo de exemplo de compartilhamento de cookie

O exemplo ilustra o cookie compartilhando entre três aplicativos que usam a autenticação de cookie:

| Projeto                             | Descrição |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | Aplicativo de páginas Razor do Core ASP.NET sem usar o ASP.NET Core Identity |
| CookieAuthWithIdentity.Core         | Aplicativo MVC do ASP.NET Core com ASP.NET Core Identity |
| CookieAuthWithIdentity.NETFramework | Aplicativo MVC do ASP.NET Framework com o ASP.NET Identity |

Instruções:

1. Execute o aplicativo CookieAuth.Core. Registre-se um usuário. O aplicativo autentica o usuário quando o usuário está registrado. Desconectar o usuário.
1. Na mesma sessão do navegador, execute o aplicativo de CookieAuthWithIdentity.Core. Registre o mesmo usuário como usado com o aplicativo de núcleo. O aplicativo autentica o usuário quando o usuário está registrado. Desconectar o usuário.
1. Na mesma sessão do navegador, execute o aplicativo de CookieAuthWithIdentity.NETFramework. Registre o mesmo usuário como usado com outros aplicativos. O aplicativo autentica o usuário quando o usuário está registrado. Desconectar o usuário.
1. A entrada do usuário para qualquer um dos três aplicativos. O cookie de autenticação é compartilhado entre os aplicativos. Observe que o usuário é conectado automaticamente aos dois aplicativos.
1. Desconectar o usuário de qualquer um dos aplicativos. Observe que o usuário é automaticamente desconectado de dois aplicativos.

Este exemplo demonstra os recursos descritos a [compartilhar cookies entre aplicativos](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) tópico.
