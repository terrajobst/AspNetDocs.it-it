> Alcuni comandi non sono supportati se l'app usa SQLite come archivio dati di identità. A causa delle limitazioni del motore di database, `Alter` comandi generano l'eccezione seguente:
>
> "System. NotSupportedException: SQLite non supporta questa operazione di migrazione". 
>
> Per aggirare l'operazione, eseguire Code First migrazioni sul database per modificare le tabelle.
