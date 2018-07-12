> Alguns comandos não têm suporte se o aplicativo usa o SQLite como seu armazenamento de dados de identidade. Devido a limitações no mecanismo de banco de dados, `Alter` comandos geram a exceção a seguir:
>
> "System. NotSupportedException: SQLite não dá suporte a essa operação de migração." 
>
> Como uma solução alternativa, execute migrações do Code First no banco de dados para alterar as tabelas.
