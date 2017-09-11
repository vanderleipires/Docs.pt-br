A tabela a seguir detalha os parâmetros dos geradores de código do ASP.NET Core:

| Parâmetro               | Descrição|
| ----------------- | ------------ |
| -m  | O nome do modelo. |
| -dc  | O contexto de dados. |
| -udl | Use o layout padrão. |
| -outDir | O caminho da pasta de saída relativa para criar as exibições. |
| --referenceScriptLibraries | Adiciona `_ValidationScriptsPartial` para editar e criar páginas |

Use a opção `h` para obter ajuda sobre o comando `aspnet-codegenerator razorpage`:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Testar o aplicativo

* Executar o aplicativo e acrescentar `/Movies` à URL no navegador (`http://localhost:port/movies`).
* Teste o link **Criar**.

 ![Criar página](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Teste os links **Editar**, **Detalhes** e **Excluir**.

Se você receber o seguinte erro, verifique se você executou migrações e atualizou o banco de dados:

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```