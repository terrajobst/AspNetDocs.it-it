---
title: Minore, Sass e Font Awesome in ASP.NET Core
author: ardalis
description: Informazioni su come usare meno, Sass e Font Awesome nelle applicazioni ASP.NET Core.
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065558"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Minore, Sass e Font Awesome in ASP.NET Core

Di [Steve Smith](https://ardalis.com/)

Se si desidera applicare uno stile e l'esperienza complessiva, gli utenti delle applicazioni web hanno sempre più ripongono grandi aspettative. Applicazioni web moderne spesso sfruttano set completo di strumenti e Framework per definire e gestire l'aspetto in modo coerente. I framework come [Bootstrap](http://getbootstrap.com/) può fare molta strada verso la definizione di un set comune di stili e opzioni di layout per i siti web. Tuttavia, la maggior parte dei siti non è semplice anche trarre vantaggio dalla possibilità di definire e mantenere stili e i file di foglio di stile CSS in modo efficace, nonché di facile accesso alle icone non di immagine che consentono di rendere più intuitiva interfaccia del sito. In questi casi i linguaggi e strumenti che supportino [minore](http://lesscss.org/) e [Sass](http://sass-lang.com/), e le librerie, ad esempio [Font Awesome](http://fontawesome.io/), sono disponibili in.

## <a name="css-preprocessor-languages"></a>Lingue per il preprocessore CSS

Le lingue che vengono compilate in altri linguaggi, per migliorare l'esperienza di utilizzo con il linguaggio sottostante, sono detti i preprocessori. Ci sono due i preprocessori più diffusi per CSS: Meno e Sass.  Questi preprocessori aggiungere funzionalità a CSS, ad esempio il supporto per le variabili e le regole annidate, di cui migliorare la manutenibilità di fogli di stile di complesso e di grandi dimensioni. Come linguaggio di CSS è molto semplice, mancanza di supporto anche per essere semplice come variabili e ciò tende a rendere i file CSS ripetitive e troppo grandi. Aggiunta di funzionalità del linguaggio reale programmazione tramite i preprocessori consentono di ridurre la duplicazione e offrire una migliore organizzazione di regole di stile. Visual Studio offre supporto predefinito per entrambi minore e Sass, nonché le estensioni che consentono di migliorare ulteriormente l'esperienza di sviluppo quando si lavora con questi linguaggi.

Un rapido esempio del modo in cui i preprocessori possono migliorare la leggibilità e facilità di gestione delle informazioni di stile, prendere in considerazione il CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Usa minore, ciò può essere riscritto per escludere tutti i duplicati, usando un *mixin* (così chiamati perché consente di "mix in" proprietà da una classe o un set di regole in un altro):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>less

Le esecuzioni per il preprocessore meno CSS con Node. js. Per installare minore, usare Node Package Manager (npm) da un prompt dei comandi (-g significa "globale"):

```console
npm install -g less
```

Se si usa Visual Studio, è possibile iniziare con meno aggiungendo uno o più file di Less al progetto e quindi configurando Gulp o Grunt per elaborarli in fase di compilazione. Aggiungere un *stili* cartella al progetto, quindi aggiungere un nuovo file denominato Less *main.less* a questa cartella.

![Aggiungere file less](less-sass-fa/_static/add-less-file.png)

Una volta aggiunta, la struttura di cartelle avrà un aspetto simile al seguente:

![struttura di cartelle](less-sass-fa/_static/folder-structure.png)

A questo punto è possibile aggiungere alcuni stili di base per il file, che verrà compilato in CSS e distribuito nella cartella wwwroot da Gulp.

Modificare *main.less* per includere il contenuto seguente, che crea una tavolozza dei colori semplice da un solo colore di base.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` e l'altro @-prefixed gli elementi sono variabili. Ognuno di essi rappresenta un colore. Ad eccezione di `@base`, vengono completate usando le funzioni di colore: schiarire scurire e rotazione. Rendere più chiari e più scura è pressoché ciò che vi aspettereste; selezione viene modificata la tonalità di colore da un numero di gradi (circa il selettore di colore). Il processore di Less è abbastanza intelligente da ignorare le variabili che non vengono usate, pertanto per illustrare il funzionano di queste variabili, è necessario utilizzarli da qualche parte. Le classi `.baseColor`, illustra i valori calcolati di ogni variabile nel file CSS in produzione e così via.

### <a name="get-started"></a>Introduzione

Creare un **File di configurazione npm** (*package. JSON*) nella cartella del progetto e modificarlo per fare riferimento `gulp` e `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Installare le dipendenze in un prompt dei comandi nella cartella del progetto o in Visual Studio **Esplora soluzioni** (**dipendenze > npm > ripristino pacchetti**).

```console
npm install
```

![Visual Studio il ripristino dei pacchetti](less-sass-fa/_static/restore-packages.png)

Nella cartella del progetto, creare un **File di configurazione Gulp** (*gulpfile. js*) per definire il processo automatico.  Aggiungere una variabile nella parte superiore del file per rappresentare minore e un'attività da eseguire minore:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Aprire il **Esplora esecuzione attività** (**visualizzazione > altri Windows > Esplora esecuzione attività**). Tra le attività, dovrebbe essere una nuova attività denominata `less`. Si potrebbe essere necessario aggiornare la finestra.

Eseguire il `less` attività ed è visualizzato un output simile a quello mostrato di seguito:

![runner di minore attività](less-sass-fa/_static/less-task-runner.png)

Il *wwwroot/css* cartella ora contiene un nuovo file *Main. CSS*:

![css principale creato](less-sass-fa/_static/main-css-created.png)

Aprire *Main. CSS* e viene visualizzato qualcosa di simile al seguente:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Aggiungere una pagina HTML semplice per la *wwwroot* cartelle e riferimenti *Main. CSS* per visualizzare la tavolozza dei colori in azione.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

È possibile visualizzare che di 180 gradi girare su `@base` utilizzati per produrre `@background` comportato la rotellina colori opposti colore della `@base`:

![esempio meno test](less-sass-fa/_static/less-test-screenshot.png)

Minore fornisce inoltre supporto per le regole annidate, nonché query sui supporti annidati. Ad esempio, definire le gerarchie annidate, come i menu possono comportare dettagliato delle regole CSS come queste:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

In teoria tutte le regole di stile di visualizzazione correlate verranno posizionate all'interno del file CSS, ma in pratica non c'è niente applicare questa regola, ad eccezione convenzione e forse i commenti di blocco.

La definizione di queste regole stesso utilizzando minore aspetto simile al seguente:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Si noti che in questo caso, tutti gli elementi subordinati di `nav` sono contenuti all'interno del relativo ambito. Non è più disponibile qualsiasi ripetizione di elementi padre (`nav`, `li`, `a`), e il conteggio delle righe totali è stato eliminato anche (anche se alcune delle che è il risultato dell'inserimento di valori nella stessa riga nel secondo esempio). Può essere molto utile, livello di organizzazione, per visualizzare tutte le regole per un determinato elemento dell'interfaccia utente all'interno di un ambito limitato in modo esplicito, in questo caso impostare dal resto del file da parentesi graffe.

Il `&` sintassi è una funzionalità meno selettore con & che rappresenta il padre di selector corrente. In questo caso, all'interno di una {...} blocco `&` rappresenta un `a` tag e pertanto `&:link` equivale a `a:link`.

Query sui supporti, estremamente utili nella creazione di progettazioni reattive, può anche contribuire attivamente ai ripetizione e la complessità in CSS. Minore consente query sui supporti annidata all'interno delle classi, in modo che l'intera definizione della classe non deve essere ripetuto all'interno di diversi principale `@media` elementi. Ad esempio, ecco CSS per un menu reattivo:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Ciò può essere meglio definito in meno come:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Un'altra funzionalità minore che abbiamo già visto è il supporto di operazioni matematiche, che consente gli attributi di stile a partire da variabili predefinite. In questo modo l'aggiornamento correlati stili molto più semplici, poiché la variabile di base può essere modificata e tutti i valori dipendenti modificata automaticamente.

File CSS, in particolare per i siti di grandi dimensioni (e in particolare se vengono usate query sui supporti), tendono a raggiungere dimensioni notevoli nel corso del tempo, rendendo difficile da gestire lavora con esse. File di less possono essere specificati separatamente, quindi uniti utilizzando `@import` direttive. Minore è anche utilizzabile per importare i singoli file CSS, anche se lo si desidera.

*"Mixins"* può accettare parametri e minore supporta una logica condizionale sotto forma di protezioni mixin, che forniscono una modalità dichiarativa per definire quando applicate determinate combinazioni. Un uso comune per le protezioni mixin per regolare i colori di base come la luce o scura del colore di origine. Dato un mixin che accetta un parametro per il colore, una clausola guard mixin può essere utilizzata per modificare il mixin basata su tale colore:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Dato il corrente `@base` pari a `#663333`, meno lo script genererà il codice CSS seguente:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Minore fornisce una serie di funzionalità aggiuntive, ma dovrebbero fornire un'idea della potenza di questo linguaggio di pre-elaborazione.

## <a name="sass"></a>Sass

Sass è simile a un valore inferiore, che fornisce supporto per molte delle stesse funzionalità, ma con una sintassi leggermente diversa. Viene compilata usando Ruby, piuttosto che JavaScript e pertanto ha requisiti di installazione diverso. Lingua il Sass originale non usa le parentesi graffe oppure punti e virgola, ma invece definita ambito utilizzando lo spazio vuoto e il rientro. Nella versione 3 del Sass, è stata introdotta una nuova sintassi, **SCSS** "Sassy CSS ("). SCSS è simile a CSS, in quanto Ignora spazi vuoti e i livelli di rientro e Usa invece un punto e virgola e le parentesi graffe.

Per installare il Sass, in genere sarebbe prima di tutto installare Ruby (preinstallato in macOS) e quindi eseguire:

```console
gem install sass
```

Tuttavia, se si esegue Visual Studio, è possibile iniziare a con il Sass in gran parte esattamente come si farebbe con minore. Aprire *package. JSON* e aggiungere il pacchetto "gulp sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

A questo punto, modificare *gulpfile. js* per aggiungere una variabile sass e un'attività per compilare i file il Sass e inserire i risultati nella cartella wwwroot:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

A questo punto è possibile aggiungere file il Sass *main2.scss* per il *stili* cartella nella radice del progetto:

![aggiungere file scss](less-sass-fa/_static/add-scss-file.png)

Aprire *main2.scss* e aggiungere quanto segue:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Salvare tutti i file. A questo punto quando si aggiornano **Task Runner Explorer**, viene visualizzato un `sass` attività. Eseguirlo ed esaminare i */wwwroot/css* cartella. È ora disponibile un' *main2.css* file, con questo contenuto:

```css
body {
    background-color: #CC0000;
}
```

Sass supporta l'annidamento in gran parte uguale era che minore non, offrendo vantaggi simili. I file possono essere suddivisi in base (funzione) e incluso utilizzando il `@import` direttiva:

```sass
@import 'anotherfile';
```

Sass supporta "mixins", usando il `@mixin` parola chiave per definirli e `@include` per includerli, come in questo esempio dalla [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Oltre a "mixins", Sass supporta anche il concetto di ereditarietà, consentendo una classe estendere un altro. È concettualmente simile a un mixin, ma risulta meno codice CSS. Viene eseguito utilizzando il `@extend` (parola chiave). Per provare "mixins", il comando seguente per aggiungere il *main2.scss* file:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Esaminare l'output nella *main2.css* dopo aver eseguito la `sass` delle attività nella **Task Runner Explorer**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Si noti che tutte le proprietà comuni del mixin avviso vengono ripetute in ogni classe. Il mixin ha un buon lavoro utile per eliminare la duplicazione in fase di sviluppo, ma che si sta creando ancora CSS con molta, risultati duplicati di dimensioni superiori al file CSS necessari - potenziali problemi di prestazioni.

Sostituire ora il mixin avviso con un `.alert` classe e modificare `@include` al `@extend` (ricordando di estendere `.alert`, non `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Eseguire ancora una volta il Sass ed esaminare il file CSS risultante:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Ora le proprietà sono definite solo il numero di volte in base alle esigenze e meglio CSS viene generato.

Sass include anche le funzioni e operazioni per la logica condizionale, simile a un valore inferiore. In effetti, le funzionalità di due lingue sono molto simili.

## <a name="less-or-sass"></a>Minore o il Sass?

Non è ancora presente alcuna il consenso per quanto riguarda se in genere è preferibile usare minore o il Sass (o persino se preferisce il Sass originale o la sintassi SCSS più recente entro il Sass). Probabilmente la decisione più importante consiste nel **usare uno di questi strumenti**, anziché semplicemente-codifica manuale dei file CSS. Dopo aver apportato che Decision Trees, entrambi minore e il Sass costituiscono un'ottima scelta.

## <a name="font-awesome"></a>Font Awesome

Oltre ai preprocessori CSS, un'altra straordinaria risorsa per le applicazioni web moderne lo stile è Font Awesome. Font Awesome è un toolkit che fornisce oltre 500 icone vettore scalabile che possono essere liberamente utilizzate nelle applicazioni web. È stato originariamente progettato per funzionare con Bootstrap, ma non ha dipendenze in tale framework o in qualsiasi libreria JavaScript.

Il modo più semplice per iniziare a usare Font Awesome consiste nell'aggiungere un riferimento a esso, usando il percorso di rete (CDN di) distribuzione di contenuti pubblici:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

È possibile anche aggiungerlo al progetto di Visual Studio aggiungendolo alle "dipendenze" nella *bower. JSON*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Dopo aver creato un riferimento a Font Awesome in una pagina, è possibile aggiungere le icone all'applicazione tramite l'applicazione di classi Font Awesome, in genere il prefisso "fa-", per gli elementi HTML inline (ad esempio `<span>` o `<i>`).  Ad esempio, è possibile aggiungere le icone da semplici elenchi e menu usando codice simile al seguente:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Questa operazione produce quanto segue nel browser - si noti l'icona accanto a ogni elemento:

![elenco icone](less-sass-fa/_static/list-icons-screenshot.png)

È possibile visualizzare un elenco completo delle icone disponibili qui:

http://fontawesome.io/icons/

## <a name="summary"></a>Riepilogo

Applicazioni web moderne richiedono sempre più reattive e fluide progettazioni che sono pulita, intuitiva e facile da usare da svariati dispositivi. Gestire la complessità dei fogli di stile CSS necessarie per raggiungere questi obiettivi è meglio effettuare un mi piace per il preprocessore che utilizzano meno risorse o il Sass. Inoltre, Toolkit, come Font Awesome fornire rapidamente le icone note ai menu di navigazione testuale e pulsanti, migliorando l'utente complessiva esperienza utente dell'applicazione.
