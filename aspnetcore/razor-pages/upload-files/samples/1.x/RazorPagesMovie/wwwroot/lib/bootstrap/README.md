---
ms.openlocfilehash: f60f934934ae9e16bbd1174959d99aecdbb5833d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57025108"
---
--

[![Slack](https://bootstrap-slack.herokuapp.com/badge.svg)](https://bootstrap-slack.herokuapp.com)
![Versione Bower](https://img.shields.io/bower/v/bootstrap.svg)
[![Versione npm](https://img.shields.io/npm/v/bootstrap.svg)](https://www.npmjs.com/package/bootstrap)
[![Stato di compilazione](https://img.shields.io/travis/twbs/bootstrap/master.svg)](https://travis-ci.org/twbs/bootstrap)
[![Stato di devDependency](https://img.shields.io/david/dev/twbs/bootstrap.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![NuGet](https://img.shields.io/nuget/v/bootstrap.svg)](https://www.nuget.org/packages/Bootstrap)
[![Stato del test Selenium](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

Bootstrap è un framework front-end potente, elegante e intuitivo per lo sviluppo Web semplice e immediato, creato da [Mark Otto](https://twitter.com/mdo) e [Jacob Thornton](https://twitter.com/fat) e gestito dal [team di base](https://github.com/orgs/twbs/people) con il supporto e il coinvolgimento su vasta scala della community.

Per iniziare, vedere <http://getbootstrap.com>.


## <a name="table-of-contents"></a>Sommario

* [Avvio rapido](#quick-start)
* [Bug e richieste di funzionalità](#bugs-and-feature-requests)
* [Documentazione](#documentation)
* [Contributi al codice](#contributing)
* [Community](#community)
* [Controllo delle versioni](#versioning)
* [Autori](#creators)
* [Copyright e licenza](#copyright-and-license)


## <a name="quick-start"></a>Avvio rapido

Sono disponibili diverse opzioni di avvio rapido:

* [Download della versione più recente](https://github.com/twbs/bootstrap/archive/v3.3.7.zip).
* Clonazione del repository: `git clone https://github.com/twbs/bootstrap.git`.
* Installazione con [Bower](http://bower.io): `bower install bootstrap`.
* Installazione con [npm](https://www.npmjs.com): `npm install bootstrap@3`.
* Installazione con [Meteor](https://www.meteor.com): `meteor add twbs:bootstrap`.
* Installazione con [Composer](https://getcomposer.org): `composer require twbs/bootstrap`.

Leggere la [pagina introduttiva](http://getbootstrap.com/getting-started/) per informazioni sul contenuto del framework, oltre a modelli, esempi e molto altro ancora.

### <a name="whats-included"></a>Contenuto del download

All'interno del download sono disponibili i file e le directory seguenti, che raggruppano in modo logico le risorse comuni e forniscono variazioni compilate e minimizzate. Si vedrà una schermata simile alla seguente:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

Sono disponibili CSS e JS compilati (`bootstrap.*`), oltre a CSS e JS compilati e minimizzati (`bootstrap.min.*`). Sono inclusi anche [mapping di origine](https://developer.chrome.com/devtools/docs/css-preprocessors) CSS (`bootstrap.*.map`) per l'uso con gli strumenti di sviluppo di determinati browser. Sono disponibili tipi di carattere da Glyphicons, oltre al tema Bootstrap facoltativo.


## <a name="bugs-and-feature-requests"></a>Bug e richieste di funzionalità

Per segnalare un bug o richiedere una funzionalità, leggere prima le [linee guida per la segnalazione dei problemi](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker) ed eseguire una ricerca tra i problemi risolti ed esistenti. Se il problema o l'idea non è stata ancora segnalata, [creare una nuova segnalazione](https://github.com/twbs/bootstrap/issues/new).

Le **richieste di funzionalità devono riguardare [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev),** dal momento che Bootstrap v3 è attualmente in modalità di manutenzione e non prevede l'introduzione di nuove funzionalità. Gli sforzi del team di sviluppo sono tutti concentrati su Bootstrap v4.


## <a name="documentation"></a>Documentazione

La documentazione di Bootstrap, inclusa in questo repository nella directory radice, viene compilata con [Jekyll](http://jekyllrb.com) e ospitata pubblicamente nelle pagine di GitHub all'indirizzo <http://getbootstrap.com>. I documenti possono anche essere eseguiti in locale.

### <a name="running-documentation-locally"></a>Esecuzione della documentazione in locale

1. Se necessario, [installare Jekyll](http://jekyllrb.com/docs/installation) e altre dipendenze di Ruby con `bundle install`.
   **Nota per gli utenti di Windows:** Lettura [questa Guida non ufficiale](http://jekyll-windows.juthilo.com/) ottenere Jekyll attivo e in esecuzione senza problemi.
2. Dalla directory `/bootstrap` radice, eseguire `bundle exec jekyll serve` nella riga di comando.
4. Aprire `http://localhost:9001` nel browser.

Altre informazioni sull'uso di Jekyll sono disponibili nella relativa [documentazione](http://jekyllrb.com/docs/home/).

### <a name="documentation-for-previous-releases"></a>Documentazione per le versioni precedenti

La documentazione per v2.3.2 è attualmente disponibile all'indirizzo <http://getbootstrap.com/2.3.2/> in concomitanza con la transizione degli utenti a Bootstrap 3.

Anche le [versioni precedenti](https://github.com/twbs/bootstrap/releases) e la relativa documentazione sono disponibili per il download.


## <a name="contributing"></a>Contributi al codice

Leggere le [linee guida per i collaboratori](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md), che includono anche istruzioni per la segnalazione di problemi, la codifica di standard e note di sviluppo.

Inoltre, se la richiesta pull contiene funzionalità o patch JavaScript, è necessario includere gli [unit test pertinenti](https://github.com/twbs/bootstrap/tree/master/js/tests). Tutti gli HTML e i CSS devono essere conformi alla [guida per la codifica](https://github.com/mdo/code-guide), gestita da [Mark Otto](https://github.com/mdo).

**Bootstrap v3 non prevede l'introduzione di nuove funzionalità.** Attualmente è in modalità di manutenzione e gli sforzi del team di sviluppo sono tutti concentrati su [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev), il framework di prossima generazione. Le richieste pull mirate ad aggiungere nuove funzionalità, anziché a correggere i bug, devono riguardare [Bootstrap v4 (ramo git `v4-dev`)](https://github.com/twbs/bootstrap/tree/v4-dev).

Le preferenze per l'editor sono disponibili in [editorconfig](https://github.com/twbs/bootstrap/blob/master/.editorconfig) per un uso immediato nei principali editor di testo. Scopri di più e scarica i plugin all'indirizzo <http://editorconfig.org>.


## <a name="community"></a>Community

È possibile rimanere sempre aggiornati sugli sviluppi di Bootstrap e chattare con i gestori del progetto e i membri della community.

* Seguire [@getbootstrap su Twitter](https://twitter.com/getbootstrap).
* Leggere e sottoscrivere il [blog ufficiale di Bootstrap](http://blog.getbootstrap.com).
* Partecipare alla [chat room ufficiale di Slack](https://bootstrap-slack.herokuapp.com).
* Chattare con altri utenti di Bootstrap in IRC, sul server `irc.freenode.net`, nel canale `##bootstrap`.
* Il supporto per l'implementazione è disponibile in Stack Overflow (con tag [`twitter-bootstrap-3`](https://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).
* Gli sviluppatori devono usare la parola chiave `bootstrap` nei pacchetti che modificano o aggiungono funzionalità di Bootstrap durante la distribuzione tramite [npm](https://www.npmjs.com/browse/keyword/bootstrap) o meccanismi di recapito analoghi per la massima individuabilità.


## <a name="versioning"></a>Controllo delle versioni

Per la trasparenza del ciclo di rilascio e l'impegno nel mantenimento della compatibilità con le versioni precedenti, Bootstrap viene gestito secondo le [linee guida per il Versionamento Semantico](http://semver.org/), a cui si cerca di aderire il più possibile.

Vedere la [sezione relativa alle versioni del progetto GitHub](https://github.com/twbs/bootstrap/releases) per i log delle modifiche per ogni versione di rilascio di Bootstrap. I post sugli annunci del rilascio delle versioni sul [blog ufficiale di Bootstrap](http://blog.getbootstrap.com) contengono un riepilogo delle modifiche più importanti apportate in ogni versione.


## <a name="creators"></a>Autori

**Mark Otto**

* <https://twitter.com/mdo>
* <https://github.com/mdo>

**Jacob Thornton**

* <https://twitter.com/fat>
* <https://github.com/fat>


## <a name="copyright-and-license"></a>Copyright e licenza

Copyright per il codice e la documentazione 2011-2016 Twitter, Inc. Codice rilasciato secondo i [termini della licenza MIT](https://github.com/twbs/bootstrap/blob/master/LICENSE). Documentazione rilasciata secondo i [termini della licenza Creative Commons](https://github.com/twbs/bootstrap/blob/master/docs/LICENSE).
