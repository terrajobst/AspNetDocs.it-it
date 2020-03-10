---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profili, temi e Web part | Microsoft Docs
author: microsoft
description: In ASP.NET 2,0 sono state apportate modifiche importanti alla configurazione e alla strumentazione. La nuova API di configurazione ASP.NET consente di apportare modifiche alla configurazione...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: cf5c45781be6d003d28c6aa27efa08032579a6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587234"
---
# <a name="profiles-themes-and-web-parts"></a>Profili, temi e Web part

[Microsoft](https://github.com/microsoft)

> In ASP.NET 2,0 sono state apportate modifiche importanti alla configurazione e alla strumentazione. La nuova API di configurazione ASP.NET consente di apportare modifiche di configurazione a livello di codice. Sono inoltre disponibili molte nuove impostazioni di configurazione che consentono nuove configurazioni e strumentazione.

ASP.NET 2,0 rappresenta un miglioramento sostanziale nell'area dei siti Web personalizzati. Oltre alle funzionalità di appartenenza già analizzate, i profili ASP.NET, i temi e le web part migliorano significativamente la personalizzazione nei siti Web.

## <a name="aspnet-profiles"></a>Profili ASP.NET

I profili ASP.NET sono simili alle sessioni. La differenza è che un profilo è persistente, mentre una sessione viene persa quando il browser viene chiuso. Un'altra differenza sostanziale tra le sessioni e i profili è che i profili sono fortemente tipizzati e pertanto forniscono IntelliSense durante il processo di sviluppo.

Un profilo è definito nel file di configurazione del computer o nel file Web. config dell'applicazione. Non è possibile definire un profilo in un file Web. config di sottocartelle. Il codice seguente definisce un profilo in cui archiviare i visitatori del sito Web e il cognome.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Il tipo di dati predefinito per una proprietà del profilo è System. String. Nell'esempio precedente non è stato specificato alcun tipo di dati. Pertanto, le proprietà FirstName e LastName sono entrambe di tipo String. Come indicato in precedenza, le proprietà del profilo sono fortemente tipizzate. Il codice seguente aggiunge una nuova proprietà per Age di tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

I profili vengono in genere usati con l'autenticazione basata su form ASP.NET. Se usato in combinazione con l'autenticazione basata su form, ogni utente dispone di un profilo separato associato al rispettivo ID utente. Tuttavia, è anche possibile consentire l'uso di profili in un'applicazione anonima usando l'elemento &lt;anonymousIdentification&gt; nel file di configurazione insieme all'attributo **AllowAnonymous** come illustrato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Quando un utente anonimo Esplora il sito, ASP.NET crea un'istanza di **ProfileCommon** per l'utente. Questo profilo usa un ID univoco archiviato in un cookie nel browser per identificare l'utente come visitatore univoco. In questo modo, è possibile archiviare le informazioni sul profilo per gli utenti che eseguono l'esplorazione in modo anonimo.

## <a name="profile-groups"></a>Gruppi di profili

È possibile raggruppare le proprietà dei profili. Raggruppando le proprietà, è possibile simulare più profili per un'applicazione specifica.

La configurazione seguente configura una proprietà FirstName e LastName per due gruppi; Acquirenti e prospettive.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

È quindi possibile impostare le proprietà in un particolare gruppo come indicato di seguito:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Archiviazione di oggetti complessi

Finora, gli esempi trattati hanno archiviato tipi di dati semplici in un profilo. È anche possibile archiviare tipi di dati complessi in un profilo specificando il metodo di serializzazione usando l'attributo **Serializes** come indicato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

In questo caso, il tipo è PurchaseInvoice. La classe PurchaseInvoice deve essere contrassegnata come serializzabile e può contenere un numero qualsiasi di proprietà. Se, ad esempio, PurchaseInvoice dispone di una proprietà denominata **NumItemsPurchased**, è possibile fare riferimento a tale proprietà nel codice nel modo seguente:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Ereditarietà del profilo

È possibile creare un profilo da usare in più applicazioni. Creando una classe del profilo che deriva da ProfileBase, è possibile riutilizzare un profilo in più applicazioni utilizzando l'attributo **Inherits** , come illustrato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

In questo caso, la classe **PurchasingProfile** avrà un aspetto simile al seguente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Provider di profili

I profili ASP.NET usano il modello provider. Il provider predefinito archivia le informazioni in un database SQL Server Express nella cartella app\_data dell'applicazione Web usando il provider SqlProfileProvider. Se il database non esiste, ASP.NET lo creerà automaticamente quando il profilo tenterà di archiviare le informazioni.

In alcuni casi, tuttavia, potrebbe essere necessario sviluppare un provider di profili personalizzato. La funzionalità profilo ASP.NET consente di usare facilmente provider diversi.

Si crea un provider di profili personalizzato quando:

- È necessario archiviare le informazioni sul profilo in un'origine dati, ad esempio in un database FoxPro o in un database Oracle, che non è supportata dai provider di profili inclusi nel .NET Framework.
- È necessario gestire le informazioni del profilo utilizzando uno schema del database diverso da quello utilizzato dai provider inclusi nel .NET Framework. Un esempio comune è che si desidera integrare le informazioni del profilo con i dati utente in un database di SQL Server esistente.

### <a name="required-classes"></a>Classi obbligatorie

Per implementare un provider di profili, è necessario creare una classe che erediti la classe astratta System. Web. profile. ProfileProvider. La classe astratta **ProfileProvider** eredita a sua volta la classe astratta System. Configuration. SettingsProvider che eredita la classe astratta System. Configuration. provider. ProviderBase. A causa di questa catena di ereditarietà, oltre ai membri necessari della classe **ProfileProvider** , è necessario implementare i membri obbligatori delle classi **SettingsProvider** e **ProviderBase** .

Le tabelle seguenti descrivono le proprietà e i metodi che è necessario implementare dalle classi astratte **ProviderBase**, **SettingsProvider**e **ProfileProvider** .

### <a name="providerbase-members"></a>Membri di ProviderBase

| **Membro** | **Descrizione** |
| --- | --- |
| Initialize (metodo) | Accetta come input il nome dell'istanza del provider e un NameValueCollection di impostazioni di configurazione. Consente di impostare le opzioni e i valori delle proprietà per l'istanza del provider, inclusi i valori specifici dell'implementazione e le opzioni specificate nella configurazione del computer o nel file Web. config. |

### <a name="settingsprovider-members"></a>Membri di SettingsProvider

| **Membro** | **Descrizione** |
| --- | --- |
| Proprietà ApplicationName | Nome dell'applicazione archiviato con ciascun profilo. Il provider di profili usa il nome dell'applicazione per archiviare le informazioni sul profilo separatamente per ogni applicazione. Questo consente a più applicazioni ASP.NET di usare la stessa origine dati senza conflitti se lo stesso nome utente viene creato in applicazioni diverse. In alternativa, più applicazioni ASP.NET possono condividere un'origine dati del profilo specificando lo stesso nome dell'applicazione. |
| Metodo GetPropertyValues | Accetta come input un oggetto SettingsContext e un oggetto SettingsPropertyCollection. **SettingsContext** fornisce informazioni sull'utente. È possibile utilizzare le informazioni come chiave primaria per recuperare le informazioni sulle proprietà del profilo per l'utente. Usare l'oggetto **SettingsContext** per ottenere il nome utente e se l'utente è autenticato o anonimo. **SettingsPropertyCollection** contiene una raccolta di oggetti SettingsProperty. Ogni oggetto **SettingsProperty** fornisce il nome e il tipo della proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e se la proprietà è di sola lettura. Il metodo **GetPropertyValues** popola un SettingsPropertyValueCollection con oggetti SettingsPropertyValue basati sugli oggetti **SettingsProperty** forniti come input. I valori dell'origine dati per l'utente specificato vengono assegnati alle proprietà PropertyValue per ogni oggetto **SettingsPropertyValue** e viene restituita l'intera raccolta. La chiamata al metodo aggiorna anche il valore LastActivityDate del profilo utente specificato alla data e all'ora correnti. |
| Metodo sepropertyvalues | Accetta come input un oggetto **SettingsContext** e un oggetto **SettingsPropertyValueCollection** . **SettingsContext** fornisce informazioni sull'utente. È possibile utilizzare le informazioni come chiave primaria per recuperare le informazioni sulle proprietà del profilo per l'utente. Usare l'oggetto **SettingsContext** per ottenere il nome utente e se l'utente è autenticato o anonimo. **SettingsPropertyValueCollection** contiene una raccolta di oggetti **SettingsPropertyValue** . Ogni oggetto **SettingsPropertyValue** fornisce il nome, il tipo e il valore della proprietà, nonché informazioni aggiuntive, ad esempio il valore predefinito per la proprietà e se la proprietà è di sola lettura. Il metodo **Sepropertyvalues** aggiorna i valori delle proprietà del profilo nell'origine dati per l'utente specificato. La chiamata al metodo aggiorna inoltre i valori **LastActivityDate** e LastUpdatedDate per il profilo utente specificato alla data e all'ora correnti. |

### <a name="profileprovider-members"></a>Membri di ProfileProvider

| **Membro** | **Descrizione** |
| --- | --- |
| Metodo DeleteProfiles | Accetta come input una matrice di stringhe di nomi utente ed Elimina dall'origine dati tutte le informazioni sul profilo e i valori delle proprietà per i nomi specificati dove il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Se l'origine dati supporta le transazioni, si consiglia di includere tutte le operazioni di eliminazione in una transazione e di eseguire il rollback della transazione e generare un'eccezione in caso di esito negativo di un'operazione di eliminazione. |
| Metodo DeleteProfiles | Accetta come input una raccolta di oggetti ProfileInfo ed Elimina dall'origine dati tutte le informazioni sul profilo e i valori delle proprietà per ogni profilo in cui il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni DELETE in una transazione ed eseguire il rollback della transazione e generare un'eccezione in caso di esito negativo di un'operazione di eliminazione. |
| Metodo DeleteInactiveProfiles | Accetta come input un valore ProfileAuthenticationOption e un oggetto DateTime ed Elimina dall'origine dati tutte le informazioni sul profilo e i valori delle proprietà in cui la data dell'ultima attività è minore o uguale alla data e all'ora specificate e in cui il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Il parametro **ProfileAuthenticationOption** specifica se devono essere eliminati solo i profili anonimi, solo i profili autenticati o tutti i profili. Se l'origine dati supporta le transazioni, è consigliabile includere tutte le operazioni DELETE in una transazione ed eseguire il rollback della transazione e generare un'eccezione in caso di esito negativo di un'operazione di eliminazione. |
| Metodo GetAllProfiles | Accetta come input un valore **ProfileAuthenticationOption** , un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un Integer che verrà impostato sul numero totale di profili. Restituisce un oggetto ProfileInfoCollection che contiene oggetti **ProfileInfo** per tutti i profili nell'origine dati in cui il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Il parametro **ProfileAuthenticationOption** specifica se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. I risultati restituiti dal metodo **GetAllProfiles** sono limitati dai valori dell'indice e della dimensione della pagina. Il valore delle dimensioni della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica la pagina dei risultati da restituire, dove 1 identifica la prima pagina. Il parametro per il totale dei record è un parametro out (è possibile usare **ByRef** in Visual Basic) impostato sul numero totale di profili. Se, ad esempio, l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, l'oggetto **ProfileInfoCollection** restituito contiene il sesto fino al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce. |
| Metodo GetAllInactiveProfiles | Accetta come input un valore **ProfileAuthenticationOption** , un oggetto **DateTime** , un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un Integer che verrà impostato sul numero totale di profili. Restituisce un oggetto **ProfileInfoCollection** che contiene oggetti **ProfileInfo** per tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale al valore **DateTime** specificato e il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Il parametro **ProfileAuthenticationOption** specifica se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. I risultati restituiti dal metodo **GetAllInactiveProfiles** sono limitati dai valori dell'indice e della dimensione della pagina. Il valore delle dimensioni della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica la pagina dei risultati da restituire, dove 1 identifica la prima pagina. Il parametro per il totale dei record è un parametro out (è possibile usare **ByRef** in Visual Basic) impostato sul numero totale di profili. Se, ad esempio, l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, l'oggetto **ProfileInfoCollection** restituito contiene il sesto fino al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce. |
| Metodo FindProfilesByUserName | Accetta come input un valore **ProfileAuthenticationOption** , una stringa contenente un nome utente, un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un Integer che verrà impostato sul numero totale di profili. Restituisce un oggetto **ProfileInfoCollection** che contiene oggetti **ProfileInfo** per tutti i profili nell'origine dati in cui il nome utente corrisponde al nome utente specificato e dove il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Il parametro **ProfileAuthenticationOption** specifica se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. Se l'origine dati supporta funzionalità di ricerca aggiuntive, ad esempio caratteri jolly, è possibile fornire funzionalità di ricerca più estese per i nomi utente. I risultati restituiti dal metodo **FindProfilesByUserName** sono limitati dai valori dell'indice e della dimensione della pagina. Il valore delle dimensioni della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica la pagina dei risultati da restituire, dove 1 identifica la prima pagina. Il parametro per il totale dei record è un parametro out (è possibile usare **ByRef** in Visual Basic) impostato sul numero totale di profili. Se, ad esempio, l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, l'oggetto **ProfileInfoCollection** restituito contiene il sesto fino al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce. |
| Metodo FindInactiveProfilesByUserName | Accetta come input un valore **ProfileAuthenticationOption** , una stringa che contiene un nome utente, un oggetto **DateTime** , un numero intero che specifica l'indice della pagina, un numero intero che specifica le dimensioni della pagina e un riferimento a un Integer che verrà impostato sul numero totale di profili. Restituisce un oggetto **ProfileInfoCollection** che contiene oggetti **ProfileInfo** per tutti i profili nell'origine dati in cui il nome utente corrisponde al nome utente specificato, in cui la data dell'ultima attività è minore o uguale al valore **DateTime**specificato e dove il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Il parametro **ProfileAuthenticationOption** specifica se devono essere restituiti solo profili anonimi, solo profili autenticati o tutti i profili. Se l'origine dati supporta funzionalità di ricerca aggiuntive, ad esempio caratteri jolly, è possibile fornire funzionalità di ricerca più estese per i nomi utente. I risultati restituiti dal metodo **FindInactiveProfilesByUserName** sono limitati dai valori dell'indice e della dimensione della pagina. Il valore delle dimensioni della pagina specifica il numero massimo di oggetti **ProfileInfo** da restituire in **ProfileInfoCollection**. Il valore di indice della pagina specifica la pagina dei risultati da restituire, dove 1 identifica la prima pagina. Il parametro per il totale dei record è un parametro out (è possibile usare **ByRef** in Visual Basic) impostato sul numero totale di profili. Se, ad esempio, l'archivio dati contiene 13 profili per l'applicazione e il valore di indice della pagina è 2 con una dimensione di pagina pari a 5, l'oggetto **ProfileInfoCollection** restituito contiene il sesto fino al decimo profilo. Il valore totale dei record viene impostato su 13 quando il metodo restituisce. |
| Metodo GetNumberOfInActiveProfiles | Accetta come input un valore **ProfileAuthenticationOption** e un oggetto **DateTime** e restituisce un conteggio di tutti i profili nell'origine dati in cui la data dell'ultima attività è minore o uguale al valore **DateTime** specificato e dove il nome dell'applicazione corrisponde al valore della proprietà **ApplicationName** . Il parametro **ProfileAuthenticationOption** specifica se devono essere conteggiati solo i profili anonimi, solo i profili autenticati o tutti i profili. |

### <a name="applicationname"></a>ApplicationName

Poiché i provider di profili archiviano le informazioni sul profilo separatamente per ogni applicazione, è necessario assicurarsi che lo schema dati includa il nome dell'applicazione e che anche le query e gli aggiornamenti includano il nome dell'applicazione. Ad esempio, il comando seguente viene usato per recuperare un valore di proprietà da un database in base al nome utente e se il profilo è anonimo e assicura che il valore **ApplicationName** venga incluso nella query.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temi ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Che cosa sono i temi di ASP.NET 2,0?

Uno degli aspetti più importanti di un'applicazione Web è un aspetto coerente in tutto il sito. Gli sviluppatori ASP.NET 1. x usano in genere Cascading Style Sheets (CSS) per implementare un aspetto coerente. I temi di ASP.NET 2,0 migliorano significativamente in CSS perché offrono allo sviluppatore di ASP.NET la possibilità di definire l'aspetto dei controlli server ASP.NET e degli elementi HTML. I temi ASP.NET possono essere applicati a singoli controlli, a una pagina Web specifica o a un'intera applicazione Web. I temi usano una combinazione di file CSS, un file di interfaccia facoltativo e una directory di immagini facoltative se sono necessarie immagini. Il file di interfaccia controlla l'aspetto visivo dei controlli server ASP.NET.

## <a name="where-are-themes-stored"></a>Dove vengono archiviati i temi?

La posizione in cui vengono archiviati i temi differisce in base al relativo ambito. I temi che possono essere applicati a qualsiasi applicazione vengono archiviati nella cartella seguente:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema specifico di una particolare applicazione viene archiviato in una directory `App\_Themes\<Theme\_Name>` nella radice del sito Web.

> [!NOTE]
> Un file di interfaccia deve modificare solo le proprietà del controllo server che influiscono sull'aspetto.

Un tema globale è un tema che può essere applicato a qualsiasi applicazione o sito Web in esecuzione sul server Web. Questi temi vengono archiviati per impostazione predefinita nella directory ASP. NETClientfiles\Themes all'interno della directory V2. x. xxxxx. In alternativa, è possibile spostare i file del tema nella cartella ASPNET\_client/System\_Web/[Version]/Themes/[Theme\_Name] nella radice del sito Web.

I temi specifici dell'applicazione possono essere applicati solo all'applicazione in cui risiedono i file. Questi file vengono archiviati nella directory `App\_Themes/<theme\_name>` nella radice del sito Web.

## <a name="the-components-of-a-theme"></a>Componenti di un tema

Un tema è costituito da uno o più file CSS, un file skin facoltativo e una cartella images facoltativa. I file CSS possono essere qualsiasi nome desiderato (ad esempio, default. CSS o Theme. CSS e così via) e devono trovarsi nella radice della cartella dei temi. I file CSS vengono utilizzati per definire classi e attributi CSS comuni per selettori specifici. Per applicare una delle classi CSS a un elemento Page, viene usata la proprietà **CssClass** .

Il file di interfaccia è un file XML che contiene le definizioni delle proprietà per i controlli server ASP.NET. Il codice riportato di seguito è un esempio di file di interfaccia.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

La **Figura 1** seguente mostra una pagina di ASP.NET di piccole dimensioni visualizzata senza un tema applicato. La **Figura 2** Mostra lo stesso file con un tema applicato. Il colore di sfondo e il colore del testo sono configurati tramite un file CSS. L'aspetto del pulsante e della casella di testo viene configurato usando il file di interfaccia indicato in precedenza.

![Nessun tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: nessun tema

![Tema applicato](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema applicato

Il file di interfaccia elencato sopra definisce un'interfaccia predefinita per tutti i controlli TextBox e i pulsanti. Ciò significa che ogni controllo TextBox e pulsante inserito in una pagina verrà utilizzato per questo aspetto. È anche possibile definire un'interfaccia che può essere applicata a istanze specifiche di questi controlli usando la proprietà **SkinID** del controllo.

Il codice seguente definisce un'interfaccia per un controllo Button. Solo i controlli Button con una proprietà **SkinID** di **goButton** verranno importati sull'aspetto dell'interfaccia.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

È possibile avere una sola interfaccia predefinita per ogni tipo di controllo server. Se sono necessarie interfacce aggiuntive, è necessario usare la proprietà SkinID.

## <a name="applying-themes-to-pages"></a>Applicazione di temi a pagine

Un tema può essere applicato usando uno dei metodi seguenti:

- Nel &lt;pagine&gt; elemento del file Web. config
- Nella direttiva @Page di una pagina
- A livello di codice

## <a name="applying-a-theme-in-the-configuration-file"></a>Applicazione di un tema nel file di configurazione

Per applicare un tema nel file di configurazione delle applicazioni, usare la sintassi seguente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Il nome del tema specificato deve corrispondere al nome della cartella temi. Questa cartella può essere presente in una qualsiasi delle posizioni citate in precedenza in questo corso. Se si tenta di applicare un tema che non esiste, si verificherà un errore di configurazione.

## <a name="applying-a-theme-in-the-page-directive"></a>Applicazione di un tema nella direttiva della pagina

È anche possibile applicare un tema nella direttiva @ Page. Questo metodo consente di usare un tema per una pagina specifica.

Per applicare un tema nella direttiva @Page, usare la sintassi seguente:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Ancora una volta, il tema specificato deve corrispondere alla cartella del tema come indicato in precedenza. Se si tenta di applicare un tema che non esiste, si verificherà un errore di compilazione. In Visual Studio viene inoltre evidenziato l'attributo e viene notificato che non esiste alcun tema di questo tipo.

## <a name="applying-a-theme-programmatically"></a>Applicazione di un tema a livello di codice

Per applicare un tema a livello di codice, è necessario specificare la proprietà **Theme** per la pagina nella **pagina\_metodo PreInit** .

Per applicare un tema a livello di codice, usare la sintassi seguente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

È necessario applicare il tema nel metodo PreInit a causa del ciclo di vita della pagina. Se viene applicato dopo tale punto, il tema delle pagine sarà già stato applicato dal runtime e una modifica a tale punto è troppo tardi per il ciclo di vita. Se si applica un tema che non esiste, viene eseguita un'operazione **HttpException** . Quando viene applicato un tema a livello di codice, si verificherà un avviso di compilazione se per qualsiasi controllo server è specificata una proprietà SkinID. Questo avviso indica che nessun tema è stato applicato in modo dichiarativo e può essere ignorato.

## <a name="exercise-1--applying-a-theme"></a>Esercizio 1: applicazione di un tema

In questo esercizio verrà applicato un tema ASP.NET a un sito Web.

> [!IMPORTANT]
> Se si usa Microsoft Word per immettere le informazioni in un file di interfaccia, assicurarsi di non sostituire le virgolette regolari con le virgolette intelligenti. Le virgolette intelligenti provocheranno problemi con i file di interfaccia.

1. Creare un nuovo sito Web ASP.NET.
2. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
3. Scegliere file di configurazione Web dall'elenco di file e fare clic su Aggiungi.
4. Fare clic con il pulsante destro del mouse sul progetto in Esplora soluzioni e scegliere Aggiungi nuovo elemento.
5. Scegliere file di interfaccia e fare clic su Aggiungi.
6. Fare clic su Sì quando viene chiesto se si vuole inserire il file nella cartella temi\_app.
7. Fare clic con il pulsante destro del mouse sulla cartella SkinFile all'interno della cartella temi\_app Esplora soluzioni e scegliere Aggiungi nuovo elemento.
8. Scegliere il foglio di stile dall'elenco dei file e fare clic su Aggiungi. Sono ora disponibili tutti i file necessari per implementare il nuovo tema. Tuttavia, Visual Studio ha denominato la cartella dei temi SkinFile. Fare clic con il pulsante destro del mouse sulla cartella e modificare il nome in CoolTheme.
9. Aprire il file SkinFile. Skin e aggiungere il codice seguente alla fine del file: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Salvare il file SkinFile. Skin.
11. Aprire il foglio di stile CSS.
12. Sostituire tutto il testo al suo interno con quanto segue: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Salvare il file StyleSheet. CSS.
14. Aprire la pagina default. aspx.
15. Aggiungere un controllo TextBox e un controllo Button.
16. Salvare la pagina. A questo punto, esplorare la pagina default. aspx. Dovrebbe essere visualizzato come un Web Form normale.
17. Aprire il file Web. config.
18. Aggiungere quanto segue direttamente sotto il tag `<system.web>` di apertura: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Salvare il file Web. config. A questo punto, esplorare la pagina default. aspx. Dovrebbe essere visualizzato con il tema applicato.
20. Se non è già aperta, aprire la pagina default. aspx in Visual Studio.
21. Selezionare il pulsante.
22. Modificare la proprietà **SkinID** in goButton. Si noti che Visual Studio fornisce un elenco a discesa con valori SkinID validi per un controllo Button.
23. Salvare la pagina. Ora visualizzare di nuovo la pagina nel browser. Il pulsante dovrebbe ora indicare "go" e deve avere un aspetto più ampio.

Utilizzando la proprietà **SkinID** , è possibile configurare facilmente interfacce diverse per diverse istanze di un particolare tipo di controllo server.

## <a name="the-stylesheettheme-property"></a>Proprietà StyleSheetTheme

Fino ad ora, abbiamo parlato solo dell'applicazione di temi usando la proprietà Theme. Quando si usa la proprietà Theme, il file skin esegue l'override di tutte le impostazioni dichiarative per i controlli server. Ad esempio, nell'esercizio 1 è stato specificato un SkinID di "goButton" per il controllo Button e il testo del pulsante è stato modificato in "go". Si può notare che la proprietà Text del pulsante nella finestra di progettazione è stata impostata su "button", ma il tema è stato sottoposto a override. Il tema eseguirà sempre l'override delle impostazioni delle proprietà nella finestra di progettazione.

Se si vuole essere in grado di eseguire l'override delle proprietà definite nel file skin del tema con le proprietà specificate nella finestra di progettazione, è possibile usare la proprietà **StyleSheetTheme** invece della proprietà Theme. La proprietà StyleSheetTheme è uguale alla proprietà Theme, ad eccezione del fatto che non esegue l'override di tutte le impostazioni di proprietà esplicite come la proprietà Theme.

Per verificarne il funzionamento, aprire il file Web. config dal progetto nell'esercizio 1 e modificare l'elemento `<pages>` come indicato di seguito:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

A questo punto, esplorare la pagina default. aspx. si noterà che il controllo Button ha nuovamente una proprietà Text di "button". Ciò è dovuto al fatto che l'impostazione della proprietà esplicita nella finestra di progettazione esegue l'override della proprietà Text impostata da goButton SkinID.

## <a name="overriding-themes"></a>Override di temi

È possibile eseguire l'override di un tema globale applicando un tema con lo stesso nome nella cartella temi\_app dell'applicazione. Tuttavia, il tema non viene applicato in uno scenario di override reale. Se il runtime rileva i file del tema nella cartella app\_themes, il tema verrà applicato usando tali file e ignorerà il tema globale.

La proprietà StyleSheetTheme è sottoponibile a override e può essere sottoposta a override nel codice nel modo seguente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web part

ASP.NET Web part è un set integrato di controlli per la creazione di siti Web che consentono agli utenti finali di modificare il contenuto, l'aspetto e il comportamento delle pagine Web direttamente da un browser. Le modifiche possono essere applicate a tutti gli utenti del sito o a singoli utenti. Quando gli utenti modificano pagine e controlli, è possibile salvare le impostazioni per mantenere le preferenze personali dell'utente nelle sessioni future del browser, una funzionalità denominata personalizzazione. Queste funzionalità Web part significano che gli sviluppatori possono consentire agli utenti finali di personalizzare un'applicazione Web in modo dinamico, senza l'intervento di uno sviluppatore o un amministratore.

Utilizzando il set di controllo Web part, gli sviluppatori possono consentire agli utenti finali di:

- Personalizzare il contenuto della pagina. Gli utenti possono aggiungere nuovi controlli Web part a una pagina, rimuoverli, nasconderli o ridurli a icona come le finestre normali.
- Personalizzare il layout di pagina. Gli utenti possono trascinare un controllo Web part in un'area diversa di una pagina o modificarne l'aspetto, le proprietà e il comportamento.
- Esportare e importare i controlli. Gli utenti possono importare o esportare le impostazioni di controllo Web part per l'uso in altre pagine o siti, mantenendo le proprietà, l'aspetto e persino i dati nei controlli. In questo modo si riducono le richieste di immissione e configurazione dei dati agli utenti finali.
- Creare connessioni. Gli utenti possono stabilire connessioni tra i controlli in modo che, ad esempio, un controllo Chart possa visualizzare un grafico per i dati in un controllo del titolo di borsa. Gli utenti potrebbero personalizzare non solo la connessione, ma l'aspetto e i dettagli del modo in cui il controllo Chart Visualizza i dati.
- Gestire e personalizzare le impostazioni a livello di sito. Gli utenti autorizzati possono configurare le impostazioni a livello di sito, determinare chi può accedere a un sito o a una pagina, impostare l'accesso in base al ruolo ai controlli e così via. Ad esempio, un utente in un ruolo amministrativo potrebbe impostare un controllo Web part per la condivisione da parte di tutti gli utenti e impedire agli utenti che non sono amministratori di personalizzare il controllo condiviso.

In genere si utilizza Web part in uno dei tre modi seguenti: creazione di pagine che utilizzano controlli Web part, creazione di singoli controlli Web part o creazione di applicazioni Web complete e personalizzabili, ad esempio un portale.

## <a name="page-development"></a>Sviluppo di pagine

Gli sviluppatori di pagine possono utilizzare strumenti di progettazione visivi come Microsoft Visual Studio 2005 per creare pagine che utilizzano Web part. Uno dei vantaggi dell'uso di uno strumento come Visual Studio è che il set di controllo Web part fornisce funzionalità per la creazione e la configurazione del trascinamento della selezione di controlli Web part in una finestra di progettazione visiva. È ad esempio possibile utilizzare la finestra di progettazione per trascinare una zona Web part o un controllo Web part Editor nell'area di progettazione, quindi configurare il controllo direttamente nella finestra di progettazione utilizzando l'interfaccia utente fornita dal set di controlli di Web part. In questo modo è possibile velocizzare lo sviluppo di applicazioni Web part e ridurre la quantità di codice da scrivere.

## <a name="control-development"></a>Sviluppo di controlli

È possibile utilizzare qualsiasi controllo ASP.NET esistente come controllo Web part, inclusi controlli server Web standard, controlli server personalizzati e controlli utente. Per il controllo massimo a livello di codice dell'ambiente, è anche possibile creare controlli Web part personalizzati che derivano dalla classe WebPart. Per lo sviluppo di singoli controlli di Web part, in genere si crea un controllo utente che viene usato come controllo Web part o si sviluppa un controllo Web part personalizzato.

Come esempio di sviluppo di un controllo Web part personalizzato, è possibile creare un controllo per fornire le funzionalità fornite da altri controlli server ASP.NET che possono essere utili per il pacchetto come controllo Web part personalizzabile: calendari, elenchi, informazioni finanziarie, Notizie, calcolatori, controlli Rich Text per l'aggiornamento del contenuto, griglie modificabili che si connettono a database, grafici che aggiornano dinamicamente le visualizzazioni o informazioni meteo e di viaggio. Se si fornisce una finestra di progettazione visiva con il controllo, gli sviluppatori di pagine che usano Visual Studio possono semplicemente trascinare il controllo in una zona Web part e configurarlo in fase di progettazione senza dover scrivere codice aggiuntivo.

La personalizzazione è la base della funzionalità Web part. Consente agli utenti di modificare, o personalizzare, il layout, l'aspetto e il comportamento dei controlli Web part in una pagina. Le impostazioni personalizzate sono di lunga durata: non solo durante la sessione del browser corrente (come nello stato di visualizzazione), ma anche nell'archiviazione a lungo termine, in modo che le impostazioni di un utente vengano salvate anche per le future sessioni del browser. Per impostazione predefinita, la personalizzazione è abilitata per le pagine Web part.

I componenti strutturali dell'interfaccia utente si basano sulla personalizzazione e forniscono la struttura di base e i servizi necessari per tutti i controlli Web part. Un componente strutturale dell'interfaccia utente necessario in ogni pagina Web part è il controllo WebPartManager. Sebbene non sia mai visibile, questo controllo ha l'attività critica di coordinare tutti i controlli Web part in una pagina. Ad esempio, tiene traccia di tutti i singoli controlli Web part. Gestisce le zone Web part (aree che contengono controlli Web part in una pagina) e i controlli in cui si trovano le zone. Consente inoltre di tenere traccia e controllare le diverse modalità di visualizzazione di una pagina, ad esempio browse, Connect, Edit o Catalog, e se le modifiche apportate alla personalizzazione vengono applicate a tutti gli utenti o a singoli utenti. Infine, avvia e tiene traccia delle connessioni e delle comunicazioni tra Web part controlli.

Il secondo tipo di componente strutturale dell'interfaccia utente è la zona. Le zone fungono da gestori di layout in una pagina Web part. Contengono e organizzano controlli che derivano dalla classe della parte (controlli parte) e consentono di eseguire layout di pagina modulare in orientamento orizzontale o verticale. Le zone offrono anche elementi dell'interfaccia utente comuni e coerenti (ad esempio lo stile di intestazione e piè di pagina, il titolo, lo stile del bordo, i pulsanti di azione e così via) per ogni controllo che contengono; questi elementi comuni sono noti come Chrome di un controllo. Diversi tipi specializzati di zone vengono utilizzati nelle diverse modalità di visualizzazione e con i vari controlli. I diversi tipi di zone sono descritti nella sezione Web part Essential Controls riportata di seguito.

I controlli dell'interfaccia utente Web part, che derivano tutti dalla classe della **parte** , costituiscono l'interfaccia utente primaria in una pagina di Web part. Il set di controllo Web part è flessibile e incluso nelle opzioni che fornisce per la creazione di controlli di parte. Oltre a creare controlli di Web part personalizzati, è anche possibile usare controlli server ASP.NET, controlli utente o controlli server personalizzati esistenti come controlli Web part. I controlli essenziali usati più di frequente per la creazione di Web part pagine sono descritti nella sezione successiva.

## <a name="web-parts-essential-controls"></a>Web part controlli essenziali

Il set di controllo Web part è esteso, ma alcuni controlli sono essenziali perché sono necessari per il funzionamento di Web part o perché sono i controlli usati più di frequente nelle pagine di Web part. Quando si inizia a usare Web part e si creano pagine Web part di base, è utile acquisire familiarità con i controlli Web part essenziali descritti nella tabella seguente.

| **Controllo Web part** | **Descrizione** |
| --- | --- |
| WebPartManager | Gestisce tutti i controlli Web part in una pagina. Per ogni Web part pagina è necessario un controllo **WebPartManager** (e solo uno). |
| CatalogZone | Contiene controlli CatalogPart. Usare questa zona per creare un catalogo di controlli Web part da cui gli utenti possono selezionare i controlli da aggiungere a una pagina. |
| EditorZone | Contiene i controlli EditorPart. Usare questa zona per consentire agli utenti di modificare e personalizzare Web part controlli in una pagina. |
| WebPartZone | Contiene e fornisce il layout generale per i controlli WebPart che compongono l'interfaccia utente principale di una pagina. Usare questa zona quando si creano pagine con controlli Web part. Le pagine possono contenere una o più zone. |
| ConnectionsZone | Contiene i controlli WebPartConnection e fornisce un'interfaccia utente per la gestione delle connessioni. |
| WebPart (GenericWebPart) | Esegue il rendering dell'interfaccia utente principale; la maggior parte dei Web part controlli dell'interfaccia utente rientrano in questa categoria. Per il controllo massimo a livello di codice, è possibile creare controlli Web part personalizzati che derivano dal controllo **WebPart** di base. È anche possibile usare controlli server, controlli utente o controlli personalizzati esistenti come controlli Web part. Ogni volta che uno di questi controlli viene inserito in una zona, il controllo **WebPartManager** ne esegue automaticamente il wrapping con i controlli **GenericWebPart** in fase di esecuzione, in modo che sia possibile utilizzarli con Web part funzionalità. |
| CatalogPart | Contiene un elenco di controlli di Web part disponibili che gli utenti possono aggiungere alla pagina. |
| WebPartConnection | Crea una connessione tra due controlli Web part in una pagina. La connessione definisce uno dei controlli Web part come provider (di dati) e l'altro come consumer. |
| EditorPart | Funge da classe base per i controlli dell'editor specializzati. |
| Controlli EditorPart (AppearanceEditorPart, LayoutEditorPart (, BehaviorEditorPart (e PropertyGridEditorPart) | Consenti agli utenti di personalizzare i vari aspetti dei controlli dell'interfaccia utente di Web part in una pagina |

## <a name="lab-create-a-web-part-page"></a>Lab: creare una pagina Web part

In questo Lab verrà creata una pagina Web part che renderà permanente le informazioni tramite i profili ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Creazione di una pagina semplice con Web part

In questa parte della procedura dettagliata viene creata una pagina che usa Web part controlli per visualizzare il contenuto statico. Il primo passaggio per lavorare con Web part consiste nel creare una pagina con due elementi strutturali necessari. Per prima cosa, una pagina Web Part necessita di un controllo WebPartManager per tenere traccia e coordinare tutti i controlli Web part. In secondo luogo, una pagina Web part necessita di una o più zone, che sono controlli compositi che contengono WebPart o altri controlli server e occupano un'area specificata di una pagina.

> [!NOTE]
> Non è necessario eseguire alcuna operazione per abilitare la personalizzazione del Web part; è abilitata per impostazione predefinita per il set di controlli Web part. Quando si esegue per la prima volta una pagina Web part in un sito, ASP.NET configura un provider di personalizzazioni predefinito per archiviare le impostazioni di personalizzazione degli utenti. Per ulteriori informazioni sulla personalizzazione, vedere Cenni preliminari sulla personalizzazione di Web part.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Per creare una pagina per contenere controlli Web part

1. Chiudere la pagina predefinita e aggiungere una nuova pagina al sito denominato WebPartsDemo. aspx.
2. Passa alla visualizzazione **progettazione** .
3. Dal menu **Visualizza** verificare che siano selezionate le opzioni **controlli non visivi** e **Dettagli** , in modo da visualizzare i tag e i controlli di layout che non dispongono di un'interfaccia utente.
4. Posizionare il punto di inserimento prima dei tag `<div>` nell'area di progettazione e premere INVIO per aggiungere una nuova riga. Posizionare il punto di inserimento prima del carattere di nuova riga, fare clic sul controllo elenco a discesa **formato blocco** nel menu e selezionare l'opzione **titolo 1** . Nell'intestazione aggiungere la **pagina demo Web part**testo.
5. Dalla scheda **WebParts** della casella degli strumenti trascinare un controllo **WebPartManager** sulla pagina, inserendolo subito dopo il carattere di nuova riga e prima dei tag `<div>`.   
  
   Il controllo **WebPartManager** non esegue il rendering di alcun output, quindi viene visualizzato come una casella grigia nell'area di progettazione.
6. Posizionare il punto di inserimento all'interno dei tag `<div>`.
7. Nel menu **layout** fare clic su **Inserisci tabella**, quindi creare una nuova tabella con una riga e tre colonne. Fare clic sul pulsante **delle proprietà della cella** **, selezionare dall'** elenco a discesa **allineamento verticale** , fare clic su **OK**e quindi di nuovo su **OK** per creare la tabella.
8. Trascinare un controllo WebPartZone nella colonna tabella a sinistra. Fare clic con il pulsante destro del mouse sul controllo **WebPartZone** , scegliere **Proprietà**e impostare le proprietà seguenti:   
  
   ID: SidebarZone   
  
   HeaderText: Sidebar
9. Trascinare un secondo controllo **WebPartZone** nella colonna della tabella intermedia e impostare le proprietà seguenti:   
  
   ID: MainZone   
  
   HeaderText: Main
10. Salvare il file.

La pagina dispone ora di due zone distinte che è possibile controllare separatamente. Tuttavia, nessuna zona presenta alcun contenuto, quindi la creazione di contenuto è il passaggio successivo. Per questa procedura dettagliata, si utilizzano controlli Web part che visualizzano solo contenuto statico.

Il layout di una zona Web part viene specificato da un elemento&gt; di &lt;ZoneTemplate. All'interno del modello di zona è possibile aggiungere qualsiasi controllo ASP.NET, indipendentemente dal fatto che sia un controllo Web part personalizzato, un controllo utente o un controllo server esistente. Si noti che in questo caso si usa il controllo Label e si aggiunge semplicemente un testo statico. Quando si inserisce un controllo server normale in una zona **WebPartZone** , ASP.NET considera il controllo come controllo Web part in fase di esecuzione, che Abilita Web part funzionalità del controllo.

**Per creare il contenuto per la zona principale**

1. In visualizzazione **progettazione** trascinare un controllo **Label** dalla scheda **standard** della casella degli strumenti nell'area dei contenuti della zona la cui proprietà **ID** è impostata su MainZone.
2. Passa alla visualizzazione **origine** . Si noti che è stato aggiunto un elemento &lt;ZoneTemplate&gt; per eseguire il wrapping del controllo **Label** in MainZone.
3. Aggiungere un attributo denominato **title** all'elemento &lt;ASP: Label&gt; e impostarne il valore sul contenuto. Rimuovere l'attributo Text = "label" dall'elemento &lt;ASP: Label&gt;. Tra i tag di apertura e di chiusura dell'elemento &lt;ASP: Label&gt;, aggiungere testo, ad esempio **benvenuto alla Home page** , all'interno di una coppia di &lt;H2&gt; tag di elemento. Il codice dovrebbe essere simile al seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Salvare il file.

Successivamente, creare un controllo utente che può essere aggiunto anche alla pagina come controllo Web part.

### <a name="to-create-a-user-control"></a>Per creare un controllo utente

1. Aggiungere un nuovo controllo utente Web al sito per fungere da controllo di ricerca. Deselezionare l'opzione per **inserire il codice sorgente in un file separato**. Aggiungerlo nella stessa directory della pagina WebPartsDemo. aspx e denominarlo SearchUserControl. ascx.   
  
    > [!NOTE]
    > Il controllo utente per questa procedura dettagliata non implementa la funzionalità di ricerca effettiva. viene usato solo per illustrare le funzionalità di Web part.
2. Passa alla visualizzazione **progettazione** . Dalla scheda **standard** della casella degli strumenti trascinare un controllo TextBox nella pagina.
3. Posizionare il punto di inserimento dopo la casella di testo appena aggiunta e premere INVIO per aggiungere una nuova riga.
4. Trascinare un controllo Button nella pagina della nuova riga sotto la casella di testo appena aggiunta.
5. Passa alla visualizzazione **origine** . Verificare che il codice sorgente per il controllo utente abbia un aspetto simile all'esempio seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Salvare e chiudere il file.

A questo punto è possibile aggiungere controlli Web part alla zona Sidebar. Si aggiungono due controlli all'area Sidebar, uno contenente un elenco di collegamenti e un altro che rappresenta il controllo utente creato nella procedura precedente. I collegamenti vengono aggiunti come controllo server **etichetta** standard, in modo analogo al modo in cui è stato creato il testo statico per la zona principale. Tuttavia, anche se i singoli controlli server contenuti nel controllo utente possono essere contenuti direttamente nella zona (come il controllo etichetta), in questo caso non lo sono. Ma fanno parte del controllo utente creato nella procedura precedente. Viene illustrato un modo comune per comprimere i controlli e le funzionalità aggiuntive desiderate in un controllo utente e quindi fare riferimento a tale controllo in una zona come controllo Web part.

In fase di esecuzione, il set di controllo Web part esegue il wrapping di entrambi i controlli con i controlli GenericWebPart. Quando un controllo **GenericWebPart** esegue il wrapping di un controllo server Web, il controllo parte generico è il controllo padre ed è possibile accedere al controllo server tramite la proprietà ChildControl del controllo padre. Questo utilizzo di controlli di parte generici consente ai controlli server Web standard di avere gli stessi attributi e comportamenti di base di Web part controlli che derivano dalla classe **WebPart** .

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Per aggiungere controlli Web part alla zona Sidebar

1. Aprire la pagina WebPartsDemo. aspx.
2. Passa alla visualizzazione **progettazione** .
3. Trascinare la pagina del controllo utente creata, SearchUserControl. ascx, da **Esplora soluzioni** nella zona la cui proprietà **ID** è impostata su SidebarZone e rilasciarla.
4. Salvare la pagina WebPartsDemo. aspx.
5. Passa alla visualizzazione **origine** .
6. All'interno dell'elemento &lt;ASP: WebPartZone&gt; per SidebarZone, appena sopra il riferimento al controllo utente, aggiungere un elemento &lt;ASP: Label&gt; con i collegamenti contenuti, come illustrato nell'esempio seguente. Aggiungere inoltre un attributo **title** al tag del controllo utente, con il valore **Search**, come illustrato. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Salvare e chiudere il file.

A questo punto è possibile eseguire il test della pagina visualizzandola nel browser. Nella pagina vengono visualizzate le due zone. La schermata seguente mostra la pagina.

**Web part pagina demo con due zone**

![Schermata di Web part VS Walkthrough 1](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: schermata di Web part vs Walkthrough 1

Nella barra del titolo di ogni controllo è presente una freccia verso il basso che consente di accedere a un menu dei verbi di azioni disponibili che è possibile eseguire su un controllo. Fare clic sul menu dei verbi per uno dei controlli, quindi fare clic sul verbo **Riduci a icona** e notare che il controllo è ridotto a icona. Dal menu dei verbi fare clic su **Ripristina**e il controllo torna alla dimensione normale.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Consentire agli utenti di modificare le pagine e modificare il layout

Web part consente agli utenti di modificare il layout dei controlli Web part trascinandoli da una zona a un'altra. Oltre a consentire agli utenti di spostare i controlli **WebPart** da una zona a un'altra, è possibile consentire agli utenti di modificare diverse caratteristiche dei controlli, inclusi l'aspetto, il layout e il comportamento. Il set di controllo Web part fornisce funzionalità di modifica di base per i controlli **WebPart** . Sebbene questa procedura non venga eseguita in questa procedura dettagliata, è anche possibile creare controlli editor personalizzati che consentono agli utenti di modificare le funzionalità dei controlli **WebPart** . Come per la modifica della posizione di un controllo **WebPart** , la modifica delle proprietà di un controllo si basa sulla personalizzazione di ASP.NET per salvare le modifiche apportate dagli utenti.

In questa parte della procedura dettagliata si aggiunge la possibilità per gli utenti di modificare le caratteristiche di base di qualsiasi controllo **WebPart** nella pagina. Per abilitare queste funzionalità, aggiungere un altro controllo utente personalizzato alla pagina, insieme a un elemento &lt;ASP: EditorZone&gt; e due controlli di modifica.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Per creare un controllo utente che consente la modifica del layout di pagina

1. In Visual Studio scegliere il sottomenu **nuovo** dal menu **file** e fare clic sull'opzione **file** .
2. Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **controllo utente Web**. Denominare il nuovo file DisplayModeMenu. ascx. Deselezionare l'opzione per **inserire il codice sorgente in un file separato**.
3. Fare clic su Aggiungi per creare il nuovo controllo.
4. Passa alla visualizzazione **origine** .
5. Rimuovere tutto il codice esistente nel nuovo file e incollare il codice seguente. Questo codice di controllo utente utilizza le funzionalità del set di controllo Web part che consentono a una pagina di modificare la visualizzazione o la modalità di visualizzazione e di modificare l'aspetto fisico e il layout della pagina mentre si è in determinate modalità di visualizzazione. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Salvare il file facendo clic sull'icona Salva sulla barra degli strumenti oppure scegliendo **Salva** dal menu **file** .

### <a name="to-enable-users-to-change-the-layout"></a>Per consentire agli utenti di modificare il layout

1. Aprire la pagina WebPartsDemo. aspx e passare alla visualizzazione **progettazione** .
2. Posizionare il punto di inserimento nella visualizzazione **progettazione** subito dopo il controllo **WebPartManager** aggiunto in precedenza. Aggiungere un valore di ritorno a freddo dopo il testo in modo che sia presente una riga vuota dopo il controllo **WebPartManager** . Posizionare il punto di inserimento nella riga vuota.
3. Trascinare il controllo utente appena creato (il file è denominato DisplayModeMenu. ascx) nella pagina WebPartsDemo. aspx e rilasciarlo nella riga vuota.
4. Trascinare un controllo EditorZone dalla sezione **WebParts** della casella degli strumenti alla cella rimanente della tabella aperta nella pagina WebPartsDemo. aspx.
5. Dalla sezione **WebParts** della casella degli strumenti trascinare un controllo AppearanceEditorPart e un controllo LayoutEditorPart (nel controllo **EditorZone** .
6. Passa alla visualizzazione **origine** . Il codice risultante nella cella della tabella dovrebbe essere simile al codice seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Salvare il file WebPartsDemo. aspx. È stato creato un controllo utente che consente di modificare le modalità di visualizzazione e modificare il layout di pagina ed è stato fatto riferimento al controllo nella pagina Web primaria.

È ora possibile testare la funzionalità per modificare le pagine e modificare il layout.

### <a name="to-test-layout-changes"></a>Per testare le modifiche di layout

1. Caricare la pagina in un browser.
2. Fare clic sul menu a discesa **modalità di visualizzazione** e selezionare **modifica**. Verranno visualizzati i titoli delle zone.
3. Trascinare il controllo **collegamenti personali** in base alla relativa barra del titolo dall'area laterale alla parte inferiore della zona principale. La pagina dovrebbe essere simile alla schermata seguente.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Web part pagina demo con il controllo collegamenti personali spostato

![Schermata di Web part VS Walkthrough 2](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: schermata di Web part vs Walkthrough 2

1. Fare clic sul menu a discesa **modalità di visualizzazione** e selezionare **Sfoglia**. La pagina viene aggiornata, i nomi delle zone scompaiono e il controllo **collegamenti personali** rimane dove è posizionato.
2. Per dimostrare che la personalizzazione funziona, chiudere il browser e quindi caricare nuovamente la pagina. Le modifiche apportate vengono salvate per le future sessioni del browser.
3. Scegliere **modifica**dal menu **modalità di visualizzazione** .   
  
   Ogni controllo nella pagina viene ora visualizzato con una freccia verso il basso nella barra del titolo, che contiene il menu a discesa dei verbi.
4. Fare clic sulla freccia per visualizzare il menu dei verbi nel controllo **collegamenti personali** . Fare clic sul verbo di **modifica** .   
  
   Viene visualizzato il controllo **EditorZone** che Visualizza i controlli EditorPart aggiunti.
5. Nella sezione **aspetto** del controllo di modifica modificare il **titolo** in Preferiti, usare l'elenco a discesa **tipo Chrome** per selezionare **solo titolo**e quindi fare clic su **applica**. Lo screenshot seguente mostra la pagina in modalità di modifica.

### <a name="web-parts-demo-page-in-edit-mode"></a>Web part pagina demo in modalità di modifica

![Schermata di Web part VS Walkthrough 3](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: schermata di Web part vs Walkthrough 3

1. Fare clic sul menu **modalità di visualizzazione** e selezionare **Sfoglia** per tornare alla modalità browse.
2. Il controllo ha ora un titolo aggiornato e nessun bordo, come illustrato nello screenshot seguente.

### <a name="edited-web-parts-demo-page"></a>Pagina demo Web part modificata

![Schermata di Web part VS Walkthrough 4](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: schermata di Web part vs Walkthrough 4

### <a name="adding-web-parts-at-run-time"></a>Aggiunta di Web part in fase di esecuzione

È anche possibile consentire agli utenti di aggiungere controlli Web part alla propria pagina in fase di esecuzione. A tale scopo, configurare la pagina con un catalogo di Web part, che contiene un elenco di controlli di Web part che si desidera rendere disponibili agli utenti.

**Per consentire agli utenti di aggiungere Web part in fase di esecuzione**

1. Aprire la pagina WebPartsDemo. aspx e passare alla visualizzazione **progettazione** .
2. Dalla scheda **WebPart** della casella degli strumenti trascinare un controllo CatalogZone nella colonna destra della tabella sotto il controllo **EditorZone** .   
  
   Entrambi i controlli possono trovarsi nella stessa cella della tabella perché non verranno visualizzati contemporaneamente.
3. Nel riquadro Proprietà assegnare la stringa **aggiungi Web part** alla proprietà HeaderText del controllo **CatalogZone** .
4. Dalla sezione **WebParts** della casella degli strumenti trascinare un controllo DeclarativeCatalogPart nell'area del contenuto del controllo **CatalogZone** .
5. Fare clic sulla freccia nell'angolo superiore destro del controllo **DeclarativeCatalogPart** per esporre il menu attività e quindi selezionare **modifica modelli**.
6. Dalla sezione **standard** della casella degli strumenti trascinare un controllo **FileUpload** e un controllo **Calendar** nella sezione **WebPartsTemplate** del controllo **DeclarativeCatalogPart** .
7. Passa alla visualizzazione **origine** . Esaminare il codice sorgente dell'elemento &lt;ASP: CatalogZone&gt;. Si noti che il controllo **DeclarativeCatalogPart** contiene un elemento &lt;&gt; WebPartsTemplate con i due controlli server racchiusi che è possibile aggiungere alla pagina dal catalogo.
8. Aggiungere una proprietà **title** a ognuno dei controlli aggiunti al catalogo, usando il valore stringa indicato per ogni titolo nell'esempio di codice riportato di seguito. Anche se il titolo non è una proprietà, è possibile impostare normalmente su questi due controlli server in fase di progettazione, quando un utente aggiunge questi controlli a una zona **WebPartZone** dal catalogo in fase di esecuzione, ognuno dei quali è incluso in un controllo **GenericWebPart** . In questo modo, possono fungere da controlli Web part, quindi potranno visualizzare i titoli.   
  
   Il codice per i due controlli contenuti nel controllo **DeclarativeCatalogPart** dovrebbe avere un aspetto simile al seguente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Salvare la pagina.

È ora possibile testare il catalogo.

### <a name="to-test-the-web-parts-catalog"></a>Per testare il catalogo Web part

1. Caricare la pagina in un browser.
2. Fare clic sul menu a discesa **modalità di visualizzazione** e selezionare **Catalogo**.   
  
   Viene visualizzato il catalogo denominato **aggiungi Web part** .
3. Trascinare il controllo **Preferiti** dalla zona principale alla parte superiore della zona laterale e rilasciarlo.
4. Nel catalogo **aggiungi Web part** selezionare entrambe le caselle di controllo e quindi selezionare **principale** nell'elenco a discesa contenente le zone disponibili.
5. Fare clic su **Aggiungi** nel catalogo. I controlli vengono aggiunti all'area principale. Se lo si desidera, è possibile aggiungere più istanze di controlli dal catalogo alla pagina.   
  
   Lo screenshot seguente mostra la pagina con il controllo di caricamento file e il calendario nella zona principale. 

![Controlli aggiunti alla zona principale dal catalogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Fare clic sul menu a discesa **modalità di visualizzazione** e selezionare **Sfoglia**. Il catalogo scompare e la pagina viene aggiornata.
7. Chiudere il browser. Caricare nuovamente la pagina. Le modifiche apportate vengono mantenute.
