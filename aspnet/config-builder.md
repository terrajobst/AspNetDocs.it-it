---
uid: config-builder
title: Generatori di configurazioni per ASP.NET
author: rick-anderson
description: Informazioni su come ottenere i dati di configurazione da origini diverse dai valori di Web. config da origini esterne.
ms.author: riande
ms.date: 10/29/2018
msc.type: content
ms.openlocfilehash: 5299d9ab057c3096773955a7461e77a80673ebfe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584511"
---
# <a name="configuration-builders-for-aspnet"></a>Generatori di configurazioni per ASP.NET

Di [Stephen Molloy](https://github.com/StephenMolloy) e [Rick Anderson](https://twitter.com/RickAndMSFT)

I generatori di configurazioni forniscono un meccanismo moderno e agile per le app ASP.NET per ottenere i valori di configurazione da origini esterne.

Generatori di configurazione:

* Sono disponibili in .NET Framework 4.7.1 e versioni successive.
* Fornire un meccanismo flessibile per la lettura dei valori di configurazione.
* Risolvere alcune delle esigenze di base delle app quando si spostano in un ambiente contenitore e basato sul cloud.
* Può essere usato per migliorare la protezione dei dati di configurazione mediante il disegno da origini precedentemente non disponibili, ad esempio Azure Key Vault e variabili di ambiente, nel sistema di configurazione .NET.

## <a name="keyvalue-configuration-builders"></a>Costruttori di configurazione chiave/valore

Uno scenario comune che può essere gestito dai generatori di configurazione è fornire un meccanismo di sostituzione chiave/valore di base per le sezioni di configurazione che seguono un modello chiave/valore. Il concetto .NET Framework di ConfigurationBuilders non è limitato a sezioni o modelli di configurazione specifici. Tuttavia, molti dei generatori di configurazioni in `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funzionano nel modello chiave/valore.

## <a name="keyvalue-configuration-builders-settings"></a>Impostazioni del Builder di configurazione chiave/valore

Le impostazioni seguenti si applicano a tutti i generatori di configurazione chiave/valore in `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modalità

I generatori di configurazione usano un'origine esterna di informazioni chiave/valore per popolare gli elementi chiave/valore selezionati del sistema di configurazione. In particolare, le sezioni `<appSettings/>` e `<connectionStrings/>` ricevono un trattamento speciale dai generatori di configurazione. I generatori funzionano in tre modalità:

* `Strict`: la modalità predefinita. In questa modalità, il generatore di configurazione opera solo in sezioni di configurazione basate su chiave/valore ben note. `Strict` modalità enumera ogni chiave nella sezione. Se viene trovata una chiave corrispondente nell'origine esterna:

   * I generatori di configurazioni sostituiscono il valore nella sezione di configurazione risultante con il valore dell'origine esterna.
* `Greedy`: questa modalità è strettamente correlata alla modalità di `Strict`. Anziché essere limitati alle chiavi già presenti nella configurazione originale:

  * I generatori di configurazioni aggiungono tutte le coppie chiave/valore dall'origine esterna alla sezione di configurazione risultante.

* `Expand`: opera sul codice XML non elaborato prima che venga analizzato in un oggetto sezione di configurazione. Può essere considerato come un'espansione di token in una stringa. Qualsiasi parte della stringa XML non elaborata corrispondente al modello `${token}` è un candidato per l'espansione del token. Se non viene trovato alcun valore corrispondente nell'origine esterna, il token non viene modificato. I generatori in questa modalità non sono limitati alle sezioni `<appSettings/>` e `<connectionStrings/>`.

Il markup seguente di *Web. config* Abilita [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in modalità `Strict`:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Il codice seguente legge le `<appSettings/>` e `<connectionStrings/>` mostrate nel file *Web. config* precedente:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Il codice precedente imposterà i valori della proprietà su:

* Valori nel file *Web. config* se le chiavi non sono impostate nelle variabili di ambiente.
* Valori della variabile di ambiente, se impostati.

Ad esempio, `ServiceID` conterrà:

* "Valore ServiceID da Web. config" se la variabile di ambiente `ServiceID` non è impostata.
* Valore della variabile di ambiente `ServiceID`, se impostata.

Nella figura seguente vengono illustrati i `<appSettings/>` chiavi/valori del file *Web. config* precedente impostati nell'editor dell'ambiente:

![Editor dell'ambiente](config-builder/static/env.png)

Nota: potrebbe essere necessario chiudere e riavviare Visual Studio per visualizzare le modifiche apportate alle variabili di ambiente.

### <a name="prefix-handling"></a>Gestione dei prefissi

I prefissi chiave possono semplificare l'impostazione delle chiavi perché:

* La configurazione del .NET Framework è complessa e nidificata.
* Le origini chiave/valore esterne sono comunemente Basic e flat per natura. Ad esempio, le variabili di ambiente non sono annidate.

Usare uno degli approcci seguenti per inserire sia `<appSettings/>` che `<connectionStrings/>` nella configurazione tramite le variabili di ambiente:

* Con il `EnvironmentConfigBuilder` nella modalità di `Strict` predefinita e i nomi di chiave appropriati nel file di configurazione. Il codice e il markup precedenti prendono questo approccio. Con questo approccio **non** è possibile avere chiavi denominate in modo identico in `<appSettings/>` e `<connectionStrings/>`.
* Usare due `EnvironmentConfigBuilder`s in modalità `Greedy` con prefissi e `stripPrefix`distinti. Con questo approccio, l'app può leggere `<appSettings/>` e `<connectionStrings/>` senza che sia necessario aggiornare il file di configurazione. Nella sezione successiva, [stripPrefix](#stripprefix), viene illustrato come eseguire questa operazione.
* Usare due `EnvironmentConfigBuilder`s in modalità `Greedy` con prefissi distinti. Con questo approccio non è possibile avere nomi di chiave duplicati perché i nomi delle chiavi devono essere diversi per prefisso.  Esempio:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Con il markup precedente, è possibile usare la stessa origine di chiave/valore flat per popolare la configurazione per due sezioni diverse.

Nella figura seguente vengono illustrati il `<appSettings/>` e `<connectionStrings/>` chiavi/valori del file *Web. config* precedente impostato nell'editor dell'ambiente:

![Editor dell'ambiente](config-builder/static/prefix.png)

Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel file *Web. config* precedente:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Il codice precedente imposterà i valori della proprietà su:

* Valori nel file *Web. config* se le chiavi non sono impostate nelle variabili di ambiente.
* Valori della variabile di ambiente, se impostati.

Ad esempio, usando il file *Web. config* precedente, le chiavi e i valori nell'immagine precedente dell'editor dell'ambiente e il codice precedente, vengono impostati i valori seguenti:

|  Chiave              | Value |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID dalle variabili env|
|    AppSetting_default            | AppSetting_default valore da ENV |
|       ConnStr_default         | ConnStr_default Val da ENV|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: booleano, il valore predefinito è `false`. 

Il markup XML precedente separa le impostazioni dell'app dalle stringhe di connessione, ma richiede tutte le chiavi nel file *Web. config* per usare il prefisso specificato. È ad esempio necessario aggiungere il prefisso `AppSetting` alla chiave `ServiceID` ("AppSetting_ServiceID"). Con `stripPrefix`, il prefisso non viene utilizzato nel file *Web. config* . Il prefisso è obbligatorio nell'origine del generatore di configurazione (ad esempio, nell'ambiente). Si prevede che la maggior parte degli sviluppatori utilizzerà `stripPrefix`.

Le applicazioni in genere eliminano il prefisso. Il *file Web. config* seguente rimuove il prefisso:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

Nel file *Web. config* precedente, la chiave `default` si trova sia nell'`<appSettings/>` che nella `<connectionStrings/>`.

Nella figura seguente vengono illustrati il `<appSettings/>` e `<connectionStrings/>` chiavi/valori del file *Web. config* precedente impostato nell'editor dell'ambiente:

![Editor dell'ambiente](config-builder/static/prefix.png)

Il codice seguente legge il `<appSettings/>` e `<connectionStrings/>` chiavi/valori contenuti nel file *Web. config* precedente:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Il codice precedente imposterà i valori della proprietà su:

* Valori nel file *Web. config* se le chiavi non sono impostate nelle variabili di ambiente.
* Valori della variabile di ambiente, se impostati.

Ad esempio, usando il file *Web. config* precedente, le chiavi e i valori nell'immagine precedente dell'editor dell'ambiente e il codice precedente, vengono impostati i valori seguenti:

|  Chiave              | Value |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID dalle variabili env|
|    default            | AppSetting_default valore da ENV |
|    default         | ConnStr_default Val da ENV|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: stringa, il valore predefinito è `@"\$\{(\w+)\}"`

Il comportamento `Expand` dei generatori Cerca nel codice XML non elaborato i token che hanno un aspetto simile `${token}`. La ricerca viene eseguita con l'espressione regolare predefinita `@"\$\{(\w+)\}"`. Il set di caratteri che corrisponde `\w` è più rigoroso di XML e molte origini di configurazione consentono. Usare `tokenPattern` quando sono necessari più caratteri di `@"\$\{(\w+)\}"` nel nome del token.

`tokenPattern`: stringa:

* Consente agli sviluppatori di modificare l'espressione regolare utilizzata per la corrispondenza dei token.
* Non viene eseguita alcuna convalida per assicurarsi che si tratta di un'espressione regolare ben formata e non pericolosa.
* Deve contenere un gruppo Capture. L'intera espressione regolare deve corrispondere all'intero token. La prima acquisizione deve essere il nome del token da cercare nell'origine della configurazione.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Generatori di configurazioni in Microsoft. Configuration. ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* È il più semplice dei generatori di configurazioni.
* Legge i valori dall'ambiente.
* Non dispone di opzioni di configurazione aggiuntive.
* Il valore dell'attributo `name` è arbitrario.

**Nota:** In un ambiente contenitore Windows le variabili impostate in fase di esecuzione vengono inserite solo nell'ambiente di processo EntryPoint. Le app eseguite come servizio o un processo non EntryPoint non prelevano queste variabili a meno che non vengano inserite in altro modo tramite un meccanismo nel contenitore. Per [IIS](https://github.com/Microsoft/iis-docker/pull/41)/i contenitori basati su [ASP.NET](https://github.com/Microsoft/aspnet-docker), la versione corrente di [ServiceMonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) gestisce questa operazione solo nel *DefaultAppPool* . Altre varianti di contenitori basate su Windows possono dover sviluppare un proprio meccanismo di inserimento per i processi non EntryPoint.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Non archiviare mai password, stringhe di connessione sensibili o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o il test.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Questo generatore di configurazioni fornisce una funzionalità simile a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) può essere usato nei progetti .NET Framework, ma è necessario specificare un file Secrets. In alternativa, è possibile definire la proprietà `UserSecretsId` nel file di progetto e creare il file Secrets RAW nel percorso corretto per la lettura. Per escludere le dipendenze esterne dal progetto, il file del segreto è formattato in formato XML. La formattazione XML è un dettaglio di implementazione e il formato non deve essere considerato attendibile. Se è necessario condividere un file *Secrets. JSON* con i progetti .NET Core, provare a usare [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). Il formato `SimpleJsonConfigBuilder` per .NET Core deve anche essere considerato un dettaglio di implementazione soggetto a modifiche.

Attributi di configurazione per `UserSecretsConfigBuilder`:

* `userSecretsId`: si tratta del metodo preferito per l'identificazione di un file di segreti XML. Funziona in modo simile a .NET Core, che usa una proprietà del progetto `UserSecretsId` per archiviare questo identificatore. La stringa deve essere univoca e non deve necessariamente essere un GUID. Con questo attributo, il `UserSecretsConfigBuilder` Cerca in un percorso locale noto (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) per un file Secrets che appartiene a questo identificatore.
* `userSecretsFile`: attributo facoltativo che specifica il file che contiene i segreti. Il carattere `~` può essere usato all'inizio per fare riferimento alla radice dell'applicazione. È necessario specificare questo attributo o l'attributo `userSecretsId`. Se vengono specificati entrambi, `userSecretsFile` avrà la precedenza.
* `optional`: booleano, valore predefinito `true`-impedisce un'eccezione se non è possibile trovare il file dei segreti. 
* Il valore dell'attributo `name` è arbitrario.

Il file Secrets ha il formato seguente:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) legge i valori archiviati nel [Azure Key Vault](/azure/key-vault/key-vault-whatis).

è necessario `vaultName` (il nome dell'insieme di credenziali o un URI per l'insieme di credenziali). Gli altri attributi consentono di controllare l'insieme di credenziali a cui connettersi, ma sono necessari solo se l'applicazione non è in esecuzione in un ambiente che funziona con `Microsoft.Azure.Services.AppAuthentication`. La libreria di autenticazione dei servizi di Azure viene usata per selezionare automaticamente le informazioni di connessione dall'ambiente di esecuzione, se possibile. È possibile eseguire l'override della selezione automatica delle informazioni di connessione fornendo una stringa di connessione.

* `vaultName`: obbligatorio se `uri` non è stato specificato. Specifica il nome dell'insieme di credenziali nella sottoscrizione di Azure da cui leggere le coppie chiave/valore.
* `connectionString`-una stringa di connessione utilizzabile da [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri`: consente di connettersi ad altri provider Key Vault con il valore `uri` specificato. Se non specificato, Azure (`vaultName`) è il provider dell'insieme di credenziali.
* `version`-Azure Key Vault fornisce una funzionalità di controllo delle versioni per i segreti. Se `version` viene specificato, il generatore recupera solo i segreti che corrispondono a questa versione.
* `preloadSecretNames`: per impostazione predefinita, questo generatore esegue una query su tutti i nomi delle chiavi nell' **insieme** di credenziali delle chiavi quando viene inizializzato. Per evitare la lettura di tutti i valori di chiave, impostare questo attributo su `false`. L'impostazione di questa opzione su `false` legge i segreti uno alla volta. La lettura di segreti uno alla volta può essere utile se l'insieme di credenziali consente l'accesso "Get", ma non l'accesso "list". **Nota:** Quando si usa la modalità `Greedy`, `preloadSecretNames` necessario essere `true` (impostazione predefinita).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) è un generatore di configurazioni di base che usa i file di una directory come origine dei valori. Il nome di un file è la chiave e il contenuto è il valore. Questo generatore di configurazione può essere utile quando viene eseguito in un ambiente contenitore orchestrato. Sistemi come Docker Swarm e Kubernetes forniscono `secrets` ai contenitori Windows orchestrati in questa modalità chiave per file.

Dettagli attributo:

* `directoryPath` - Obbligatorio. Specifica un percorso in cui cercare i valori. Per impostazione predefinita, i segreti Docker per Windows vengono archiviati nella directory *C:\ProgramData\Docker\secrets*
* `ignorePrefix` i file che iniziano con questo prefisso sono esclusi. Il valore predefinito è "ignore".
* `keyDelimiter`: il valore predefinito è `null`. Se specificato, il generatore di configurazioni attraversa più livelli della directory, costituendo i nomi delle chiavi con questo delimitatore. Se questo valore è `null`, il generatore di configurazione esamina solo il livello principale della directory.
* `optional`: il valore predefinito è `false`. Specifica se il generatore di configurazione deve causare errori se la directory di origine non esiste.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Non archiviare mai password, stringhe di connessione sensibili o altri dati sensibili nel codice sorgente. I segreti di produzione non devono essere usati per lo sviluppo o il test.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

I progetti .NET Core usano spesso file JSON per la configurazione. Il generatore [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) consente di usare i file JSON di .NET Core nel .NET Framework. Questo generatore di configurazioni fornisce un mapping di base da un'origine chiave/valore flat in specifiche aree chiave/valore della configurazione .NET Framework. Questo generatore di configurazione **non** fornisce le configurazioni gerarchiche. Il file di backup JSON è simile a un dizionario, non a un oggetto gerarchico complesso. È possibile utilizzare un file gerarchico a più livelli. Questo provider `flatten`la profondità aggiungendo il nome della proprietà a ogni livello utilizzando `:` come delimitatore.

Dettagli attributo:

* `jsonFile` - Obbligatorio. Specifica il file JSON da cui leggere. Il carattere `~` può essere usato all'inizio per fare riferimento alla radice dell'app.
* `optional`-Boolean, il valore predefinito è `true`. Impedisce la generazione di eccezioni se il file JSON non è stato trovato.
* `jsonMode` - `[Flat|Sectional]`. Il valore predefinito è `Flat`. Quando `jsonMode` viene `Flat`, il file JSON è un'unica origine chiave/valore flat. I `EnvironmentConfigBuilder` e `AzureKeyVaultConfigBuilder` sono anche origini chiave/valore Flat singole. Quando la `SimpleJsonConfigBuilder` viene configurata in modalità `Sectional`:

  * Il file JSON viene diviso concettualmente solo al livello principale in più dizionari.
  * Ognuno dei dizionari viene applicato solo alla sezione di configurazione che corrisponde al nome della proprietà di primo livello associato. Esempio:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementazione di un generatore di configurazione chiave/valore personalizzato

Se i generatori di configurazione non soddisfano le proprie esigenze, è possibile scriverne uno personalizzato. La classe di base `KeyValueConfigBuilder` gestisce le modalità di sostituzione e la maggior parte dei problemi di prefisso. Un progetto di implementazione deve solo:

* Ereditare dalla classe di base e implementare un'origine di base di coppie chiave/valore tramite il `GetValue` e `GetAllValues`:
* Aggiungere [Microsoft. Configuration. ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al progetto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

La classe di base `KeyValueConfigBuilder` fornisce gran parte del lavoro e del comportamento coerente nei generatori di configurazione chiave/valore.

## <a name="additional-resources"></a>Risorse aggiuntive

* [Repository GitHub di Configuration Builders](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticazione da servizio a servizio a Azure Key Vault con .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
