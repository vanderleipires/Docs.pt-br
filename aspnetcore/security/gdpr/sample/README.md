# <a name="gdpr-sample"></a>Exemplo GDPR

* Em *appSettings. JSON*, defina `CheckNotConsentNeeded` para `false` para exigir consentimento; caso contrário, definida como true ou omitir. Testar o aplicativo com `CheckNotConsentNeeded` definida como `false` e definido como `true`.
* Criar cookies essenciais e não essenciais com cada variação de `CheckConsentNeeded` e o consentimento concedido.
* Registre um usuário.
* Exclua cookies.
