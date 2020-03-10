---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Riferimento rapido all'API Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questa pagina contiene un elenco con brevi esempi degli oggetti, delle proprietà e dei metodi usati più di frequente per la programmazione Pagine Web ASP.NET con sintassi Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574333"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>Riferimento rapido API Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa pagina contiene un elenco con brevi esempi degli oggetti, delle proprietà e dei metodi usati più di frequente per la programmazione Pagine Web ASP.NET con sintassi Razor.
> 
> Le descrizioni contrassegnate con "(v2)" sono state introdotte in Pagine Web ASP.NET versione 2.
> 
> Per la documentazione di riferimento per le API, vedere la [documentazione di riferimento pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=208659) su MSDN.
> 
> ## <a name="software-versions"></a>Versioni del software
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2 e Pagine Web ASP.NET 1,0 (ad eccezione delle funzionalità contrassegnate come v2).

Questa pagina contiene informazioni di riferimento per gli elementi seguenti:

- [Classi](#Classes)
- [Dati](#Data)
- [Aiutanti](#Helpers)
- [Convalida](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Classi

### `AppState[key], AppState[index],App`

Contiene dati che possono essere condivisi da qualsiasi pagina dell'applicazione. È possibile usare la proprietà Dynamic `App` per accedere agli stessi dati, come nell'esempio seguente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Converte un valore stringa in un valore booleano (true/false). Restituisce false oppure il valore specificato se la stringa non rappresenta true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Converte un valore stringa in data/ora. Restituisce `DateTime.MinValue` o il valore specificato se la stringa non rappresenta una data/ora.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Converte un valore stringa in un valore decimale. Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Converte un valore stringa in un valore float. Restituisce 0,0 o il valore specificato se la stringa non rappresenta un valore decimale.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Converte un valore stringa in un numero intero. Restituisce 0 oppure il valore specificato se la stringa non rappresenta un numero intero.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crea un URL compatibile con il browser da un percorso di file locale, con parti di percorso aggiuntive facoltative.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Esegue il rendering del *valore* come markup HTML invece di eseguirne il rendering come output con codifica HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Restituisce true se il valore può essere convertito da una stringa al tipo specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Restituisce true se l'oggetto o la variabile non ha alcun valore.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Restituisce true se la richiesta è un POST. (Le richieste iniziali sono in genere un GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Specifica il percorso di una pagina di layout da applicare a questa pagina.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contiene i dati condivisi tra la pagina, le pagine di layout e le pagine parziali nella richiesta corrente. È possibile usare la proprietà Dynamic `Page` per accedere agli stessi dati, come nell'esempio seguente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Pagine layout) Esegue il rendering del contenuto di una pagina di contenuto che non si trova in alcuna sezione denominata.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Esegue il rendering di una pagina di contenuto usando il percorso specificato e i dati aggiuntivi facoltativi. È possibile ottenere i valori dei parametri aggiuntivi da `PageData` in base alla posizione (esempio 1) o alla chiave (esempio 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Pagine layout) Esegue il rendering di una sezione di contenuto con un nome. Impostare *obbligatorio* su false per rendere facoltativa una sezione.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Ottiene o imposta il valore di un cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Ottiene i file caricati nella richiesta corrente.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ottiene i dati che sono stati inseriti in un form (come stringhe). `Request[key]` controlla le raccolte `Request.Form` e `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Ottiene i dati specificati nella stringa di query dell'URL. `Request[key]` controlla le raccolte `Request.Form` e `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Disabilita selettivamente la convalida delle richieste per un elemento del form, un valore di stringa di query, un cookie o un valore di intestazione. La convalida della richiesta è abilitata per impostazione predefinita e impedisce agli utenti di pubblicare markup o altro contenuto potenzialmente pericoloso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Aggiunge un'intestazione del server HTTP alla risposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Memorizza nella cache l'output della pagina per un periodo di tempo specificato. Facoltativamente, impostare *scorrimento* per reimpostare il timeout in ogni accesso alla pagina e *VaryByParams* per memorizzare nella cache versioni diverse della pagina per ogni stringa di query diversa nella richiesta di pagina.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Reindirizza la richiesta del browser a una nuova posizione.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Imposta il codice di stato HTTP inviato al browser.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Scrive il contenuto dei *dati* nella risposta con un tipo MIME facoltativo.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Scrive il contenuto di un file nella risposta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Pagine layout) Definisce una sezione di contenuto con un nome.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Decodifica una stringa codificata in HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codifica una stringa per il rendering nel markup HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Restituisce il percorso fisico del server per il percorso virtuale specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Decodifica il testo da un URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codifica il testo da inserire in un URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Ottiene o imposta un valore esistente fino a quando l'utente non chiude il browser.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Visualizza una rappresentazione di stringa del valore dell'oggetto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ottiene dati aggiuntivi dall'URL (ad esempio, */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Modifica la password dell'utente specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Conferma un account utilizzando il token di conferma dell'account.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crea un nuovo account utente con il nome utente e la password specificati. Per richiedere un token di conferma, passare true per *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Ottiene l'identificatore di tipo integer per l'utente attualmente connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Ottiene il nome dell'utente attualmente connesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Genera un token di reimpostazione della password che può essere inviato tramite posta elettronica a un utente in modo che l'utente possa reimpostare la password.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Restituisce l'ID utente dal nome utente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Restituisce true se l'utente corrente ha effettuato l'accesso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Restituisce true se l'utente è stato confermato, ad esempio tramite un messaggio di posta elettronica di conferma.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Restituisce true se il nome dell'utente corrente corrisponde al nome utente specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Consente di registrare l'utente in impostando un token di autenticazione nel cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Disconnette l'utente rimuovendo il cookie del token di autenticazione.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Se l'utente non è autenticato, imposta lo stato HTTP su 401 (Non autorizzato).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Se l'utente corrente non è un membro di uno dei ruoli specificati, imposta lo stato HTTP su 401 (non autorizzato).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Se l'utente corrente non è l'utente specificato da *username*, imposta lo stato HTTP su 401 (non autorizzato).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Se il token di reimpostazione della password è valido, modifica la password dell'utente con la nuova password.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Dati

### `Database.Execute(SQLstatement [,parameters]`

Esegue *SQLStatement* (con parametri facoltativi), ad esempio INSERT, DELETE o Update e restituisce un conteggio dei record interessati.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Restituisce la colonna di identità dalla riga inserita più di recente.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Apre il file di database specificato o il database specificato utilizzando una stringa di connessione denominata dal file *Web. config* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Apre un database utilizzando la stringa di connessione. (Questo è contrario a `Database.Open`, che usa un nome di stringa di connessione).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Esegue una query sul database utilizzando *SQLStatement* (passando facoltativamente i parametri) e restituisce i risultati come raccolta.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Esegue *SQLStatement* (con parametri facoltativi) e restituisce un singolo record.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Esegue *SQLStatement* (con parametri facoltativi) e restituisce un singolo valore.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Helper

### `Analytics.GetGoogleHtml(webPropertyId)`

Esegue il rendering del codice JavaScript di Google Analytics per l'ID specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Esegue il rendering del codice JavaScript di StatCounter Analytics per il progetto specificato.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Esegue il rendering del codice JavaScript di Yahoo Analytics per l'account specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Passa una ricerca a Bing. Per specificare il sito in cui eseguire la ricerca e un titolo per la casella di ricerca, è possibile impostare le proprietà `Bing.SiteUrl` e `Bing.SiteTitle`. Queste proprietà vengono in genere impostate nella pagina *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inizializza un grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Aggiunge una legenda a un grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Aggiunge una serie di valori al grafico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Restituisce un hash per i dati specificati. L'algoritmo predefinito è `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Consente agli utenti di Facebook di creare una connessione alle pagine.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Esegue il rendering dell'interfaccia utente per il caricamento di file.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Esegue il rendering del tag del Gamer Xbox specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Esegue il rendering dell'immagine Gravatar per l'indirizzo di posta elettronica specificato.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Converte un oggetto dati in una stringa nel formato JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Converte una stringa di input con codifica JSON in un oggetto dati che è possibile scorrere o inserire in un database.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Esegue il rendering dei collegamenti di Social Networking usando il titolo e l'URL facoltativo specificati.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Associa un messaggio di errore a un campo del modulo. Usare l'helper `ModelState` per accedere a questo membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Associa un messaggio di errore a un form. Usare l'helper `ModelState` per accedere a questo membro.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Restituisce true se non sono presenti errori di convalida. Usare l'helper `ModelState` per accedere a questo membro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Esegue il rendering delle proprietà e dei valori di un oggetto e di tutti gli oggetti figlio.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Esegue il rendering del test di verifica di reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Imposta le chiavi pubbliche e private per il servizio reCAPTCHA. Queste proprietà vengono in genere impostate nella pagina *\_AppStart* .

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Restituisce il risultato del test recaptcha.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Esegue il rendering delle informazioni sullo stato di Pagine Web ASP.NET.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Esegue il rendering di un flusso Twitter per l'utente specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Esegue il rendering di un flusso di Twitter per il testo di ricerca specificato.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Esegue il rendering di un lettore video Flash per il file specificato con larghezza e altezza facoltative.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Esegue il rendering di un lettore Windows Media per il file specificato con larghezza e altezza facoltative.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Esegue il rendering di un lettore Silverlight per il file con *estensione xap* specificato con larghezza e altezza obbligatorie.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Restituisce l'oggetto specificato dalla *chiave*oppure null se l'oggetto non viene trovato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Rimuove dalla cache l'oggetto specificato in base alla *chiave* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Inserisce il *valore* nella cache sotto il nome specificato da *Key*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crea un nuovo oggetto `WebGrid` usando i dati di una query.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Esegue il rendering del markup per visualizzare i dati in una tabella HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Esegue il rendering di un cercapersone per l'oggetto `WebGrid`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Carica un'immagine dal percorso specificato.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Aggiunge l'immagine specificata come filigrana.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Aggiunge il testo specificato all'immagine.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Capovolge l'immagine orizzontalmente o verticalmente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Carica un'immagine quando viene inserita un'immagine in una pagina durante il caricamento di un file.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Ridimensiona un'immagine.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Ruota l'immagine a sinistra o a destra.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Salva l'immagine nel percorso specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Imposta la password per il server SMTP. Questa proprietà viene in genere impostata nella pagina *\_AppStart* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Invia un messaggio di posta elettronica.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Imposta il nome del server SMTP. Questa proprietà viene in genere impostata nella pagina *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Imposta il nome utente per il server SMTP. Normalmente è necessario impostare questa proprietà nella pagina *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Convalida

### `Html.ValidationMessage(field)`

V2 Esegue il rendering di un messaggio di errore di convalida per il campo specificato.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

V2 Visualizza un elenco di tutti gli errori di convalida.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

V2 Registra un elemento di input utente per il tipo di convalida specificato.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

V2 Esegue dinamicamente il rendering degli attributi della classe CSS per la convalida lato client in modo da poter formattare i messaggi di errore di convalida. È necessario fare riferimento alle librerie di script client appropriate e definire classi CSS.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

V2 Abilita la convalida sul lato client per il campo di input dell'utente. (È necessario fare riferimento alle librerie di script client appropriate).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

V2 Restituisce true se tutti gli elementi di input utente registrati per la convalida contengono valori validi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

V2 Specifica che gli utenti devono fornire un valore per l'elemento input utente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

V2 Specifica che gli utenti devono fornire valori per ognuno degli elementi di input utente. Questo metodo non consente di specificare un messaggio di errore personalizzato.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

V2 Specifica un test di convalida quando si usa il metodo `Validation.Add`.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
