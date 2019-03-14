---
ms.openlocfilehash: 56ee26dd77bb1b678b54b6776741f6547889fa08
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040418"
---
<a name="1140--2015-06-30"></a>1.14.0 / 30-06-2015
==================

## <a name="core"></a>Base
  * Rimozione metodo removeAttrs inutilizzato
  * Sostituzione regex per il metodo url
  * Rimozione parametro url non valido in $.ajax, sovrascritto da $.extend
  * Corretta gestione pulsante Annulla/Invia annidato
  * Correzione rientro
  * Refactoring di attributeRules e dataRules per condividere noramlizer
  * Metodo dataRules per convertire il valore in numero per input numerici
  * Aggiornamento metodo url per consentire URL relativi al protocollo
  * Rimozione segnaposto $.format deprecato
  * Uso di jQuery 1.7+ on/off, aggiunta del metodo destroy
  * Compatibilità IE8 modificata da .indexOf in $.inArray
  * Cast attributi di valore NaN in non definiti per Opera Mini
  * Arresto trimming del valore all'interno del metodo richiesto
  * Uso del selettore :disabled per trovare la corrispondenza con gli elementi disabilitati
  * Esclusione di alcuni tasti per evitare la riconvalida del campo
  * Nessun elemento pulsante di opzione/casella di controllo cercato nell'intero DOM
  * Generazione di errori migliori per i metodi di definizione delle regole non validi
  * Corretto errore di convalida numero
  * Correzione riferimento alla specifica whatwg
  * Stato attivo per l'elemento non valido durante la convalida di un set personalizzato di input
  * Reimpostazione stili degli elementi quando si usano metodi highlight personalizzati
  * Escape ID errore di accesso dollaro
  * Ripristino di "Ignora campi di sola lettura e disabilitati".
  * Aggiornamento collegamento nel commento per l'algoritmo di Luhn

## <a name="additionals"></a>Aggiuntivi
  * Aggiornamento dateITA per risolvere il problema di fuso orario
  * Correzione metodo di estensione per il solo periodo del metodo
  * Correzione metodo accept per trovare la corrispondenza solo con il periodo
  * Aggiornamento metodo time per consentire l'ora a una sola cifra
  * Eliminazione test non valido per il metodo notEqualTo
  * Aggiunta metodo notEqualTo
  * Uso riferimento jQuery corretto tramite `$`
  * Rimozione controllo regex inutile nel metodo iban
  * Numero CPF brasiliano

## <a name="localization"></a>Localizzazione
  * Aggiornamento messages_tr.js
  * Aggiornamento messages_sr_lat.js
  * Aggiunta spagnolo (Perù) (ES PE)
  * Aggiunta georgiano (ქართული, ge)
  * Corretto errore ortografico nella traduzione in catalano
  * Miglioramento traduzione in finlandese (fi)
  * Aggiunta impostazioni locali armeno (hy_AM)
  * Estensione traduzione in italiano (it) con il metodo per la valuta
  * Aggiunta impostazioni locali bn_BD
  * Aggiornamento impostazioni locali zh
  * Rimozione punto alla fine dei messaggi in italiano

<a name="1131--2014-10-14"></a>1.13.1 / 14-10-2014
==================

## <a name="core"></a>Base
  * Consentito uso di 0 come valore per autoCreateRanges
  * Applicazione impostazione Ignora per tutti gli elementi validationTargetFor
  * Nessun trimming del valore nei metodi min/max/rangelength
  * Escape id/nome prima dell'uso come selettore in errorsFor
  * Impostazione predefinita esplicita per l'opzione focusCleanup
  * Correzione regexp non corretto per il matcher describedby
  * Ignora campi di sola lettura e disabilitati
  * Miglioramento escape id, archiviazione id sottoposto a escape in describedby
  * Uso del valore restituito di submitHandler per consentire o impedire l'invio di moduli

## <a name="additionals"></a>Aggiuntivi
  * Aggiunta metodo postalcodeBR
  * Correzione metodo pattern quando il parametro è una stringa


<a name="1130--2014-07-01"></a>1.13.0 / 01-07-2014
==================

## <a name="all"></a>Tutti
* Aggiunta wrapper UMD plug-in

## <a name="core"></a>Base
* Rispetto di aria-describedby non di errore ed errori nascosti vuoti
* Miglioramento RegExp dateISO
* Aggiunta pulsante di opzione/casella di controllo per delegare l'evento click
* Uso di aria-describedby per elementi non di etichetta
* Registrazione di focusin, focusout e keyup anche su pulsante di opzione/casella di controllo
* Correzione normalizzazione per il valore di attributo rangelength
* Aggiornamento metodo elementValue per gestire i campi type="number"
* Uso di charAt invece della notazione di matrice sulle stringhe, per supportare IE8(?)

## <a name="localization"></a>Localizzazione
* Correzione della traduzione sk del metodo rangelength
* Aggiunta metodi finlandesi
* Corretto messaggio di convalida numero GL
* Corretto messaggio di convalida del metodo numero ES
* Aggiunta galiziano (GL)
* Corretti messaggi in francese per metodi min e max

## <a name="additionals"></a>Aggiuntivi
* Aggiunta metodo statesUS
* Correzione metodo dateITA per la gestione del bug DST
* Aggiunta metodo date per il persiano
* Aggiunta metodo postalCodeCA
* Aggiunta metodo postalcodeIT

<a name="1120--2014-04-01"></a>1.12.0 / 01-04-2014
==================

* Aggiunta test ARIA ([3d5658e](https://github.com/jzaefferer/jquery-validation/commit/3d5658e9e4825fab27198c256beed86f0bd12577))
* Aggiunta messaggi di localizzazione es-AR. ([7b30beb](https://github.com/jzaefferer/jquery-validation/commit/7b30beb8ebd218c38a55d26a63d529e16035c7a2))
* Aggiunta punti mancanti ai messaggi 'es' e 'es_AR'. ([a2a653c](https://github.com/jzaefferer/jquery-validation/commit/a2a653cb68926ca034b4b09d742d275db934d040))
* Aggiunta localizzazione in indonesiano (ID) ([1d348bd](https://github.com/jzaefferer/jquery-validation/commit/1d348bdcb65807c71da8d0bfc13a97663631cd3a))
* Aggiunta convalida numeri documenti in spagnolo NIF, NIE e CIF ([#830](https://github.com/jzaefferer/jquery-validation/issues/830), [317c20f](https://github.com/jzaefferer/jquery-validation/commit/317c20fa9bb772770bb9b70d46c7081d7cfc6545))
* Aggiunto modulo corrente al contesto della richiesta ajax remota ([0a18ae6](https://github.com/jzaefferer/jquery-validation/commit/0a18ae65b9b6d877e3d15650a5c2617a9d2b11d5))
* Aggiuntivi: Aggiornamento metodo IBAN, trimming spazi finali ([#970](https://github.com/jzaefferer/jquery-validation/issues/970), [347b04a](https://github.com/jzaefferer/jquery-validation/commit/347b04a7d4e798227405246a5de3fc57451d52e1))
* Metodo BIC: Miglioramento RegEx, {1} è sempre ridondante. Chiude gh-744 ([5cad6b4](https://github.com/jzaefferer/jquery-validation/commit/5cad6b493575e8a9a82470d17e0900c881130873))
* Bower: Aggiunta bower. JSON per la registrazione del pacchetto ([e86ccb0](https://github.com/jzaefferer/jquery-validation/commit/e86ccb06e301613172d472cf15dd4011ff71b398))
* Modifica riferimenti dollaro a 'jQuery', per compatibilità con jQuery.noConflict. Chiude gh-754 ([2049afe](https://github.com/jzaefferer/jquery-validation/commit/2049afe46c1be7b3b89b1d9f0690f5bebf4fbf68))
* nucleo: Aggiungere il campo "method" alla voce elenco errori ([89a15c7](https://github.com/jzaefferer/jquery-validation/commit/89a15c7a4b17fa2caaf4ff817f09b04c094c3884))
* nucleo: Aggiunta del supporto per messaggi generici tramite l'attributo data-msg ([5bebaa5](https://github.com/jzaefferer/jquery-validation/commit/5bebaa5c55c73f457c0e0181ec4e3b0c409e2a9d))
* nucleo: Consente gli attributi abbiano un valore pari a zero (ad esempio min = '0') ([#854](https://github.com/jzaefferer/jquery-validation/issues/854), [9dc0d1d](https://github.com/jzaefferer/jquery-validation/commit/9dc0d1dd946b2c6178991fb16df0223c76162579))
* nucleo: Disable deprecated $.format ([#755](https://github.com/jzaefferer/jquery-validation/issues/755), [bf3b350](https://github.com/jzaefferer/jquery-validation/commit/bf3b3509140ea8ab5d83d3ec58fd9f1d7822efc5))
* nucleo: Correzione supporto per più classi di errore ([c1f0baf](https://github.com/jzaefferer/jquery-validation/commit/c1f0baf36c21ca175bbc05fb9345e5b44b094821))
* nucleo: Ignora gli eventi per gli elementi ignorati ([#700](https://github.com/jzaefferer/jquery-validation/issues/700), [a864211](https://github.com/jzaefferer/jquery-validation/commit/a86421131ea69786ee9e0d23a68a54a7658ccdbf))
* nucleo: Miglioramento metodo elementValue ([6c041ed](https://github.com/jzaefferer/jquery-validation/commit/6c041edd21af1425d12d06cdd1e6e32a78263e82))
* nucleo: Verificare gli elementi ignorati handle Element () correttamente. ([3f464a8](https://github.com/jzaefferer/jquery-validation/commit/3f464a8da49dbb0e4881ada04165668e4a63cecb))
* nucleo: Passare dall'analisi dataRules allo stile specifica W3C HTML5 ([460fd22](https://github.com/jzaefferer/jquery-validation/commit/460fd22b6c84a74c825ce1fa860c0a9da20b56bb))
* nucleo: Attivare l'esito positivo su facoltativo ma ha altri validator con esito positivo ([#851](https://github.com/jzaefferer/jquery-validation/issues/851), [f93e1de](https://github.com/jzaefferer/jquery-validation/commit/f93e1deb48ec8b3a8a54e946a37db2de42d3aa2a))
* nucleo: Uso elemento semplice invece di wrapping dell'elemento ([03cd4c9](https://github.com/jzaefferer/jquery-validation/commit/03cd4c93069674db5415a0bf174a5870da47e5d2))
* Base: verifica dell'esecuzione di remote per ultimo ([#711](https://github.com/jzaefferer/jquery-validation/issues/711), [ad91b6f](https://github.com/jzaefferer/jquery-validation/commit/ad91b6f388b7fdfb03b74e78554cbab9fd8fca6f))
* Demo: Uso dell'opzione corretta nella demo multipart. ([#1025](https://github.com/jzaefferer/jquery-validation/issues/1025), [070edc7](https://github.com/jzaefferer/jquery-validation/commit/070edc7be4de564cb74cfa9ee4e3f40b6b70b76f))
* Correzione uso di $/jQuery in metodi aggiuntivi. Corregge #839 ([#839](https://github.com/jzaefferer/jquery-validation/issues/839), [59bc899](https://github.com/jzaefferer/jquery-validation/commit/59bc899e4586255a4251903712e813c21d25b3e1))
* Miglioramento traduzioni in cinese ([1a0bfe3](https://github.com/jzaefferer/jquery-validation/commit/1a0bfe32b16f8912ddb57388882aa880fab04ffe))
* Implementazione ARIA-Required iniziale ([bf3cfb2](https://github.com/jzaefferer/jquery-validation/commit/bf3cfb234ede2891d3f7e19df02894797dd7ba5e))
* Localizzazione: modifica valori di accettazione per l'estensione. Corregge #771, chiude gh-793. ([#771](https://github.com/jzaefferer/jquery-validation/issues/771), [12edec6](https://github.com/jzaefferer/jquery-validation/commit/12edec66eb30dc7e86756222d455d49b34016f65))
* Messaggi: Aggiunta localizzazione in islandese ([dc88575](https://github.com/jzaefferer/jquery-validation/commit/dc885753c8872044b0eaa1713cecd94c19d4c73d))
* Messaggi: Aggiunta punti mancanti ai messaggi 'bg', 'fr' e 'sr'. ([adbc636](https://github.com/jzaefferer/jquery-validation/commit/adbc6361c377bf6b74c35df9782479b1115fbad7))
* Messaggi: Creazione di messages_sr_lat. js ([f2f9007](https://github.com/jzaefferer/jquery-validation/commit/f2f90076518014d98495c2a9afb9a35d45d184e6))
* Messaggi: Creazione di messages_tj. js ([de830b3](https://github.com/jzaefferer/jquery-validation/commit/de830b3fd8689a7384656c17565ee92c2878d8a5))
* Messaggi: Correzione traduzione sr_lat, aggiunta spazio mancante ([880ba1c](https://github.com/jzaefferer/jquery-validation/commit/880ba1ca545903a41d8c5332fc4038a7e9a580bd))
* Messaggi: Aggiornamento messages_sr. js, correzione spazio mancante ([10313f4](https://github.com/jzaefferer/jquery-validation/commit/10313f418c18ea75f385248468c2d3600f136cfb))
* Metodi: Aggiunta metodo aggiuntivo per la valuta ([1a981b4](https://github.com/jzaefferer/jquery-validation/commit/1a981b440346620964c87ebdd0fa03246348390e))
* Metodi: Aggiunta virgolette inglesi alla rimozione punteggiatura di stripHTML ([aa0d624](https://github.com/jzaefferer/jquery-validation/commit/aa0d6241c3ea04663edc1e45ed6e6134630bdd2f))
* Metodi: Correzione metodo dateITA, evitando errori di ora legale ([279b932](https://github.com/jzaefferer/jquery-validation/commit/279b932c1267b7238e6652880b7846ba3bbd2084))
* Metodi: Localizzati metodi per impostazioni cultura cileno (es-CL) ([cf36b93](https://github.com/jzaefferer/jquery-validation/commit/cf36b933499e435196d951401221d533a4811810))
* Metodi: Aggiornare l'indirizzo di posta elettronica per l'uso di regex HTML5, rimozione metodo email2 ([#828](https://github.com/jzaefferer/jquery-validation/issues/828), [dd162ae](https://github.com/jzaefferer/jquery-validation/commit/dd162ae360639f73edd2dcf7a256710b2f5a4e64))
* Metodo pattern: Rimuovere i delimitatori, perché non includono nelle implementazioni HTML5. ([37992c1](https://github.com/jzaefferer/jquery-validation/commit/37992c1c9e2e0be8b315ccccc2acb74863439d3e))
* Limitazione validator carta di credito per includere la verifica della lunghezza. Chiude gh-772 ([f5f47c5](https://github.com/jzaefferer/jquery-validation/commit/f5f47c5c661da5b0c0c6d59d169e82230928a804))
* Aggiornamento messages_ko.js - chiude gh-715 ([5da3085](https://github.com/jzaefferer/jquery-validation/commit/5da3085ff02e0e6ecc955a8bfc3bb9a8d220581b))
* Aggiornamento messages_pt_BR.js. Chiude gh-782 ([4bf813b](https://github.com/jzaefferer/jquery-validation/commit/4bf813b751ce34fac3c04eaa2e80f75da3461124))
* Aggiornamento phonesUK e mobileUK per accettare nuovi prefissi. Chiude gh-750 ([d447b41](https://github.com/jzaefferer/jquery-validation/commit/d447b41b830dee984be21d8281ec7b87a852001d))
* Verifica CAP a nove cifre. Chiude gh-726 ([165005d](https://github.com/jzaefferer/jquery-validation/commit/165005d4b5780e22d13d13189d107940c622a76f))
* phoneUS: Aggiunta esclusioni N11. Chiude gh-861 ([519bbc6](https://github.com/jzaefferer/jquery-validation/commit/519bbc656bcb26e8aae5166d7b2e000014e0d12a))
* resetForm deve cancellare eventuali valori aria-invalid ([4f8a631](https://github.com/jzaefferer/jquery-validation/commit/4f8a631cbe84f496ec66260ada52db2aa0bb3733))
* valid(): Controllare tutti gli elementi. Corregge #791 - valid() convalida solo il primo elemento (invalid) ([#791](https://github.com/jzaefferer/jquery-validation/issues/791), [6f26803](https://github.com/jzaefferer/jquery-validation/commit/6f268031afaf4e155424ee74dd11f6c47fbb8553))

<a name="1111--2013-03-22"></a>1.11.1 / 22-03-2013
==================

  * Ripristino alla conversione dei parametri del metodo range in numeri. Chiude gh-702
  * Sostituzione della maggior parte dell'uso di PHP con gestori mockjax. Pulizia demo, aggiornamento al plug-in più recente input mascherato. Mantenimento demo captcha in PHP. Corregge #662
  * Rimozione evidenziazione codice inline da milk-demo. Corretta visualizzazione origine.
  * Correzione demo dynamic-totals mediante trimming degli spazi vuoti dal contenuto del modello prima del passaggio al costruttore jQuery
  * Correzione convalida min/max. Chiude gh-666. Corregge #648
  * Corretti 'messaggi' che si presentano come regola causando un'eccezione dopo l'aggiornamento tramite rules("add"). Chiude gh-670, corregge #624
  * Aggiunta localizzazione in coreano (ko). Chiude gh-671
  * Migliorato metodo postcode UK per filtrare più CAP non validi. Chiude #682
  * Aggiornamento messages_sv.js. Chiude #683
  * Modifica collegamento grunt al sito Web del progetto. Chiude #684
  * Spostamento metodo remoto verso il fondo dell'elenco per essere eseguito per ultimo, dopo che tutti gli altri metodi sono stati applicati a un campo. Corregge #679
  * Aggiornamento descrizione plugin.json, deve includere la parola 'validate'
  * Correzione errori ortografici
  * Correzione caricatore jQuery per l'uso del proprio percorso. Corregge le demo annidate.
  * Aggiornamento grunt-contrib-qunit per usare PhantomJS 1.8, quando installato tramite il modulo dei nodi 'phantomjs'
  * Impostazione di valid() affinché restituisca un valore booleano di 0 o 1. Corregge #109 - valid() non restituisce un valore booleano

<a name="1110--2013-02-04"></a>1.11.0 / 04-02-2013
==================

  * Rimozione cancellazione come numeri di regole `min`, `max` e `range`. Corregge #455. Chiude gh-528.
  * Aggiornamento etichette preesistenti - corregge #430, chiude gh-436
  * Correzione $.validator.format per evitare l'interpolazione di gruppo, in cui almeno IE8/9 sostituisce -bash con la corrispondenza. Corregge #614
  * Correzione regex mimetype
  * Aggiunta manifesto plug-in e aggiornamento intestazioni solo per la licenza MIT, eliminazione doppie licenze inutili (come jQuery).
  * Messaggi in ebraico: Rimossi punti alla fine delle frasi - corregge gh-568
  * Traduzione in francese per la convalida require_from_group. Corregge gh-573.
  * Impostazione gruppi come matrice o stringa - Corregge #479
  * Rimossi spazi con più tipi MIME
  * Correzione convalide data, errori di sintassi JS.
  * Rimozione supporto per i plug-in dei metadati, sostituzione con proprietà data-rule- e data-msg- (aggiunte in 907467e8)
  * Aggiunto sftp come url-pattern valido
  * Aggiunta localizzazione in malese (my)
  * Aggiornamento localizzazione/messages_hu.js
  * Rimozione focusin/focusout polyfill. Corregge #542 - l'inclusione di jquery.validate interferisce con gli eventi focusin e focusout in IE9
  * Localizzazione: Corretto errore ortografico nella traduzione in finlandese
  * Correzione demo RTM per visualizzare l'icona Non valido quando si passa da valido a non valido
  * Corretta restituzione prematura nella funzione remota che impediva la chiamata di ajax in caso di input immesso troppo rapidamente. Garantisce che la convalida remota convalidi sempre il valore più recente.
  * Annullamento correzione per #244. Corregge #521 - la convalida e-mail viene generata immediatamente quando nel campo si trova del testo.

<a name="1100--2012-09-07"></a>1.10.0 / 07-09-2012
===================

  * Corrette stringhe in francese per nowhitespace, phoneUS, phoneUK e mobileUK in base al feedback della community.
  * Ridenominazione dei file per language_REGION in base allo standard ISO_3166-1 (http://en.wikipedia.org/wiki/ISO_3166-1), per Taiwan la lingua è il cinese (zh) e la regione è Taiwan (TW)
  * Ottimizzazione criteri RegEx, specialmente per i numeri di telefono del Regno Unito.
  * Aggiunta nome della lingua per ogni file, ridenominazione codice della lingua in base allo standard ISO 639 per estone, georgiano, ucraino e cinese (http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
  * Aggiunta localizzazione in croato (HR)
  * Le traduzioni in francese esistenti sono state modificate e sono state aggiunte traduzioni in francese per i metodi aggiuntivi.
  * Unite modifiche per specificare messaggi di errore personalizzati negli attributi dei dati
  * Aggiornato regex dei numeri di telefono cellulare del Regno Unito per i nuovi numeri. Corregge #154
  * Aggiunta elemento a chiamata con esito positivo con test. Corregge #60
  * Corretto regex per metodo time aggiuntivo. Corregge #131
  * resetForm cancella ora il valore previousValue precedente negli elementi form. Corregge #312
  * Aggiunto test casella di controllo a require_from_group e modificato require_from_group per l'uso di elementValue. Corregge #359
  * Corretti problemi di risposta dataFilter in jQuery 1.5.2+. Corregge #405
  * Aggiunta demo jQuery Mobile. Corregge #249
  * Deottimizzazione findByName per correttezza. Corregge #82 - $.validator.prototype.findByName si interrompe in IE7
  * Aggiunti supporto e test per CAP Stati Uniti. Corregge #90
  * Cambiato lastElement in lastActive in keyup, convalida ignorata in scheda o elemento vuoto. Corregge #244
  * Rimossa rimozione numero da stripHtml. Corregge #2
  * Corretto conteggio non valido nella convalida remota da non valida a valida. Corregge #286
  * Aggiunta collegamento a file_input all'indice demo
  * Spostato metodo accept precedente in metodo di estensione aggiuntivo, aggiunto nuovo metodo accept per gestire il filtro mimetype standard del browser. Corregge #287 e sostituisce #369
  * Disabilita evento blur quando onfocusout è impostato su false. Aggiunto test.
  * Corretto problema di valore per pulsanti di opzione e caselle di controllo. Corregge #363
  * Aggiunto test per rangeWords e corretti regex e limiti nel metodo. Corregge #308
  * Corretta demo TinyMCE e aggiunto collegamento nella pagina della demo. Corregge #382
  * Modificato messaggio di localizzazione per min/max. Corregge #273
  * Aggiunto pseudoselettore per tipi di input di testo per correggere un problema con l'attributo di tipo vuoto predefinito. Aggiunti test e markup di test. Corregge #217
  * Corretto bug delegato per la demo dynamic-totals. Corregge #51
  * Corretto messaggio non corretto per validator alfanumerico
  * Rimosso controllo falso non corretto nell'attributo obbligatorio
  * richiesta correzione attributo per browser non html5. Corregge #301
  * Aggiunti metodi "require_from_group" e "skip_or_fill_minimum"
  * Uso codice iso corretto per lo svedese
  * Aggiornato file HTML demo per l'uso di doctype HTML5
  * Corretto problema regex per numeri decimali senza zero iniziali. Aggiunto nuovo test dei metodi. Corregge #41
  * Introduzione di un metodo elementValue che normalizza solo i valori di stringa (non influisce sul valore di matrice di selezione multipla). Corregge #116
  * Supporto per pulsanti Invia aggiunti in modo dinamico e aggiornato test case. Usa validateDelegate. Codice da PR #9
  * Correzione virgolette doppie non valide negli strumenti di test
  * Correzione metodo maxWords per includere il limite superiore e non escluderlo. Corregge #284
  * Corretto errore di grammatica nel messaggio del validator di intervalli in tedesco. Corregge #315
  * Corretta gestione di più nomi di classe per l'opzione errorClass. Test di Max Lynch. Corregge #280
  * Correzione uso di jQuery.format, deve essere $.validator.format. Corregge #329
  * Metodi per 'tutti' i numeri di telefono Regno Unito + CAP Regno Unito
  * Metodo pattern: Conversione parametro stringa in RegExp. Corregge il problema #223
  * errore di grammatica nel file di localizzazione in tedesco
  * Aggiunta localizzazione in estone per i messaggi
  * Miglioramento gestione descrizione comando nella demo themerollered
  * Aggiunta di type="text" nei campi di input senza attributo Type per conformità con qSA
  * Aggiornamento demo themerollered per l'uso della descrizione comando per visualizzare gli errori come sovrimpressione.
  * Aggiornamento demo themerollered per l'uso dell'interfaccia utente più recente di jQuery (insieme alla versione più recente di jQuery). Spostamento del codice per velocizzare il caricamento della pagina.
  * Corretto messaggio di errore min interrotto in giapponese.
  * Aggiornamento plug-in del modulo alla versione più recente. Miglioramento demo ajaxSubmit.
  * Eliminazione metodi dateDE e numberDE da classRuleSettings, residui dello spostamento nei metodi localizzati
  * Passaggio evento di invio al callback submitHandler
  * Corretto #219 - correzione valid() in elementi con dependency-callback o dependency-expression.
  * Miglioramento build per rimuovere la directory dist per garantire la compressione solo della versione corrente

<a name="190"></a>1.9.0
---
* Aggiunta localizzazione in basco (EU)
* Aggiunta localizzazione in sloveno (SL)
* Corretto problema #127 - le traduzioni in finlandese hanno : invece di ;
* Corretta localizzazione in russo, problema di sintassi secondario
* Aggiunto supporto per i tipi di input HTML5, corregge #97
* Migliorato supporto HTML5 impostando l'attributo novalidate nel modulo e leggendo l'attributo type.
* Corretto showLabel() con la rimozione di tutte le classi dall'elemento di errore. Rimozione solo di settings.validClass. Corregge #151.
* Aggiunto 'Pattern' ad additional-methods per la convalida rispetto alle espressioni regolari arbitrarie.
* Migliorato metodo di posta elettronica per non consentire il punto alla fine (valido per RFC, ma non voluto qui). Corregge #143
* Corrette traduzioni in svedese e norvegese, scambiati messaggi min/max. Corregge #181
* Corretto #184 - resetForm: deve annullare l'impostazione di lastElement
* Corretto #71 - miglioramento metodo time esistente e aggiunta metodo time12h per il formato orario 12 ore am/pm
* Corretto #177 - correzione convalida dell'input di un singolo pulsante di opzione o casella di controllo
* Corretto #189 - gli elementi :hidden ora vengono ignorati per impostazione predefinita
* Corretto #194 - richiesto perché l'attributo ha esito negativo se jQuery>=1.6 - uso di .prop invece di .attr
* Corretti #47, #39, #32 - numeri di carta di credito consentiti per contenere spazi e trattini (gli spazi sono generalmente inseriti dagli utenti).

<a name="181"></a>1.8.1
---
* Aggiunta localizzazione in thailandese (TH), corregge #85
* Aggiunta localizzazione in vietnamita (VI), grazie a Ngoc
* Corretto problema #78. Lo stile di errore/valido si applica a tutti i pulsanti di opzione nello stesso gruppo per la convalida richiesta.
* Non usare form.elements che non è più supportato in jQuery 1.6. È comunque difettoso (IE6-8: form.elements === form).

<a name="180"></a>1.8.0
---
* Migliorata localizzazione NL (http://plugins.jquery.com/node/14120)
* Aggiunta localizzazione in georgiano (GE), grazie a Avtandil Kikabidze
* Aggiunta localizzazione in serbo (SR), grazie ad Aleksandar Milovac
* Aggiunti ipv4 e ipv6 ai metodi aggiuntivi, grazie a Natal Ngétal
* Aggiunta localizzazione in giapponese (JA), grazie a Bryan Meyerovich
* Aggiunta localizzazione in catalano (CA), grazie a Xavier de Pedro
* Corrette istruzioni var mancanti all'interno di cicli for-in
* Correzione convalida remota, in cui un messaggio formattato risultava confuso (https://github.com/jzaefferer/jquery-validation/issues/11)
* Correzione di bug per la compatibilità con jQuery 1.5.1, mantenendo la compatibilità con le versioni precedenti

<a name="17"></a>1.7
---
* Aggiunta localizzazione in lituano (LT)
* Aggiunta localizzazione in greco (EL) (http://plugins.jquery.com/node/12319)
* Aggiunta localizzazione in lettone (LV) (http://plugins.jquery.com/node/12349)
* Aggiunta localizzazione in ebraico (HE) (http://plugins.jquery.com/node/12039)
* Corretta localizzazione in spagnolo (ES) (http://plugins.jquery.com/node/12696)
* Aggiunta demo themerolled interfaccia utente jQuery
* Rimosso Cmxform.js
* Corretti quattro punti e virgola mancanti (http://plugins.jquery.com/node/12639)
* Rinominato phone-method in additional-methods.js in phoneUS
* Aggiunti metodi phoneUK e mobileUK a additional-methods.js (http://plugins.jquery.com/node/12359)
* Estensione delle opzioni per evitare di modificare più moduli quando si usa il metodo rules-method in un singolo elemento (http://plugins.jquery.com/node/12411)
* Correzione di bug per la compatibilità con jQuery 1.4.2, mantenendo la compatibilità con le versioni precedenti

<a name="16"></a>1.6
---
* Aggiunta localizzazione in arabo (AR), portoghese (PTPT), persiano (FA), finlandese (FI) e bulgaro (BR)
* Aggiornata localizzazione in svedese (SE) (alcuni caratteri iso html mancanti)
* Corretto $.validator.addMethod per la corretta gestione delle stringhe vuote rispetto a quelle non definite per l'argomento del messaggio
* Corrette due variabili globali accidentali
* Migliorati min/max/rangeWords (in additional-methods.js) per rimuovere html prima del conteggio; buono per il conteggio delle parole in un editor RichText
* Aggiunta di metodi localizzati per DE, NL e PT, rimuovendo i metodi dateDE e numberDE (usare invece messages_de.js e methods_de.js con i metodi date e number)
* Corretta sincronizzazione invio modulo remoto, grazie a Matas Petrikas
* Migliorata convalida selezione interattiva, ora convalida anche tramite clic (tramite opzione o selezione, incoerente nei vari browser); non funziona in Safari, che non attiva un evento click sugli elementi select; corregge http://plugins.jquery.com/node/11520
* Aggiornato al plug-in del modulo più recente (2.36), correzione http://plugins.jquery.com/node/11487
* Binding all'evento blur per la destinazione equalTo per la riconvalida quando la destinazione cambia, corregge http://plugins.jquery.com/node/11450
* Convalida della selezione semplificata, tramite delega al metodo val() di jQuery per ottenere il valore selezionato; dovrebbe correggere http://plugins.jquery.com/node/11239
* Corretto messaggio predefinito per le cifre (http://plugins.jquery.com/node/9853)
* Corretto problema con messaggio remoto memorizzato nella cache (http://plugins.jquery.com/node/11029 e http://plugins.jquery.com/node/9351)
* Corretto punto e virgola mancante in additional-methods.js (http://plugins.jquery.com/node/9233)
* Aggiunto rilevamento automatico dei parametri di sostituzione nei messaggi, eliminando la necessità di fornire funzioni di formato (http://plugins.jquery.com/node/11195)
* Corretto problema con :filled/:blank causato da Sizzle (http://plugins.jquery.com/node/11144)
* Aggiunto metodo integer a additional-methods.js (http://plugins.jquery.com/node/9612)
* Corretto metodo errorsFor in cui for-attribute contiene caratteri che necessitano dell'escape per essere validi all'interno di un selettore (http://plugins.jquery.com/node/9611)

<a name="155"></a>1.5.5
---
* Correzione per http://plugins.jquery.com/node/8659
* Corretta virgola finale in messages_cs.js

<a name="154"></a>1.5.4
---
* Corretto bug del metodo remoto (http://plugins.jquery.com/node/8658)

<a name="153"></a>1.5.3
---
* Corretto bug correlato all'opzione wrapper, in cui tutti gli elementi predecessori corrispondenti all'opzione wrapper venivano selezionati (http://plugins.jquery.com/node/7624)
* Aggiornata demo multipart per l'uso dell'accordion dell'interfaccia utente di jQuery più recente
* Aggiunti metodi dateNL e time ad additionalMethods.js
* Aggiunta localizzazione in cinese tradizionale (Taiwan, tw) e kazaco (KK)
* Spostato jQuery.format (in precedenza String.format) in jQuery.validator.format. jQuery.format è deprecato e verrà rimosso nella versione 1.6 (vedere http://code.google.com/p/jquery-utils/issues/detail?id=15 per i dettagli)
* Puliti messages_pl.js e messages_ptbr.js (messaggi ancora definiti per max/min/rangeValue, che sono stati rimossi nella versione 1.4)
* Corretta logica booleana difettosa nel metodo plug-in valido per più elementi; ora tutti gli elementi devono essere validi per un risultato boolean-true (http://plugins.jquery.com/node/8481)
* Miglioramento $. AddMethod: Un terzo argomento messaggio non definito non sovrascrive un esistente (messaggio http://plugins.jquery.com/node/8443)
* Miglioramento dell'opzione submitHandler: Quando usato, fare clic su eventi relativi a inviano i pulsanti vengono acquisiti e il pulsante Invia viene inserito nel modulo prima di chiamare submitHandler e rimosso in seguito; mantiene (intatti i pulsanti di invio http://plugins.jquery.com/node/7183#comment-3585)
* Aggiunta opzione validClass, con impostazione predefinita "valid", che aggiunge la classe a tutti gli elementi validi, dopo la convalida (http://dev.jquery.com/ticket/2205)
* Aggiunto metodo creditcardtypes a additionalMethods.js, inclusi i test (tramite http://dev.jquery.com/ticket/3635)
* Miglioramento metodo remoto per consentire il messaggio sul lato server come stringa, true per valido o false per non valido usando il messaggio definito sul lato client (http://dev.jquery.com/ticket/3807)
* Migliorato metodo accept per accettare anche un elenco di valori delimitato da virgole in stile Drupal (http://plugins.jquery.com/node/8580)

<a name="152"></a>1.5.2
---
* Corretti messaggi in additional-methods.js per maxWords, minWords e rangeWords in modo da includere una chiamata a $.format
* Corretto valore passato ai metodi per escludere il ritorno a capo (\r), analogo a val() di jQuery
* Aggiunta localizzazione in slovacco (sk)
* Aggiunta demo per l'integrazione con le schede dell'interfaccia utente di jQuery
* Aggiunto esempio di raggruppamento delle selezioni alla demo tabs (vedere la seconda scheda, campo della data di nascita)

<a name="151"></a>1.5.1
---
* Aggiornata demo marketo per l'uso dell'opzione invalidHandler invece dell'evento invalid-form di binding
* Aggiunto esempio di integrazione di TinyMCE
* Aggiunta localizzazione in ucraino (ua)
* Convalida a lunghezza fissa per l'uso con un valore ridotto (regressione dalla versione 1.5 in cui il trimming generale prima della convalida è stato rimosso)
* Varie correzioni di lieve entità per la compatibilità con le versioni 1.2.6 e 1.3

<a name="15"></a>1,5
---
* Migliorata demo di base, con la convalida del campo di conferma della password dopo la modifica della password
* Corretta convalida di base per passare il valore di input senza trimming come primo parametro per i metodi di convalida, modificato di conseguenza; interrompe il metodo personalizzato esistente basato sul trimming
* Aggiunta localizzazione in norvegese (no), italiano (it), ungherese (hu) e rumeno (ro)
* Corretto #3195: Due difetti nella localizzazione in svedese
* Corretto #3503: Esteso Rules per accettare la proprietà dei messaggi: consente di specificare l'aggiunta di messaggi personalizzati a un elemento tramite rules ("add", {messaggi: {necessari: "Required! " } });
* Corretto #3356: Regressione da #2908 quando si usa meta-option
* Corretto #3370: Opzione di aggiunta ignoreTitle, impostata per ignorare la lettura messaggi dall'attributo title, consente di evitare problemi con barra degli strumenti di Google. valore predefinito è false per la compatibilità
* Corretto #3516: Evento invalid-form trigger anche quando è coinvolto nella convalida remota
* Aggiunta opzione invalidHandler come collegamento per bind("invalid-form", function() {})
* Corretto problema di Safari per il caricamento di un indicatore in ajaxSubmit-integration-demo (prima aggiungere al corpo, quindi nascondere)
* Aggiunto test per la convalida della carta di credito e migliorato messaggio predefinito
* Migliorata convalida remota, accettando le opzioni di pass-through a $.ajax come parametro (stringa url o opzioni, inclusa proprietà url più tutto quello che è supportato da $.ajax)

<a name="14"></a>1.4
---
* Corretto #2931, convalida degli elementi nell'ordine del documento e input di tipo type=image ignorato
* Corretto uso delle variabili $ e jQuery, ora completamente compatibili con tutte le variazioni di uso di noConflict
* Implementato #2908, che consente messaggi personalizzati tramite metadata ala class="{required:true,messages:{required:'required field'}}", aggiunti demo/custom-messages-metadata-demo.html
* Rimossi metodi deprecati minValue (min), maxValue (max), rangeValue (rangevalue), minLength (minlength), maxLength (maxlength), rangeLength (rangelength)
* Correzione della regressione #2215: Chiamata di unhighlight solo per gli elementi correnti, non tutti gli elementi
* Implementato #2989, che consente al pulsante immagine di annullare la convalida
* Corretto problema di Internet Explorer di convalida non corretta rispetto a maxlength = 0
* Aggiunta localizzazione in ceco (cs)
* Reimpostazione di validator.submitted su validator.resetForm(), che consente un ripristino completo quando necessario
* Corretto #3035, ignorando tutti gli attributi false durante la lettura delle regole (0, non definita, stringa vuota), rimossa parte della soluzione alternativa maxlength (per 0)
* Aggiunta localizzazione in olandese (nl) (#3201)

<a name="13"></a>1.3
---
* Corretto evento invalid-form, ora attivato solo quando il modulo non è valido
* Aggiunta localizzazione in spagnolo (es), russo (ru), portoghese brasiliano (ptbr), turco (tr) e polacco (pl)
* Aggiunto plug-in removeAttrs per facilitare l'aggiunta e la rimozione di più attributi
* Aggiunta opzione groups per visualizzare un singolo messaggio per più elementi, tramite groups: { arbitraryGroupName: "fieldName1 fieldName2[, fieldNameN" }
* Migliorato rules() per l'aggiunta e la rimozione di regole (statiche): rules("add", "method1[, methodN]"/{method1:param[, method_n:param]}) e rules("remove"[, "method1[, method_n]")
* Migliorata opzione rules, accetta string-list dei metodi separata da spazi, ad es. {birthdate: "required date"}
* Correzione convalida gruppo la casella di controllo con regole inline: Purché le regole siano specificate sul primo elemento, il gruppo viene ora convalidato correttamente fare clic su
* Corretto #2473, ignorando tutte le regole con un parametro esplicito di boolean-false, ad es. required:false equivale a non specificare che è richiesto (finora gestito come required:true)
* Fisso i 2424 #, con una patch modificata da #2473: I metodi che restituiscono una mancata corrispondenza di dipendenza non smettere di altre regole; più la valutazione comunque, operazione riuscita non viene applicata per i campi facoltativi
* Corretta convalida url e posta elettronica per non usare valori sottoposti a trimming
* Corretta convalida carta di credito per accettare solo cifre e trattini ("asdf" non è un numero di carta di credito valido)
* Abilitazione elementi button e di input per i pulsanti Annulla (tramite class="cancel")
* Corretto #2215: Visualizzazione del messaggio predefinito per chiamare unhighlight come parte di mostrare e nascondere i messaggi, non è più effetto collaterale visual durante la verifica di un elemento e validator. checkform estratto per convalidare un modulo senza livello dell'interfaccia utente
* Riscritti selettori personalizzati (:blank, :filled, :unchecked) con le funzioni per la compatibilità con AIR

<a name="121"></a>1.2.1
-----

* Aggregato plug-in delegato con plug-in di convalida, sempre richiesto comunque
* Migliorata convalida remota per includere parti dal plug-in ajaxQueue per la corretta sincronizzazione (nessun altro plug-in richiesto)
* Corretto stopRequest per evitare pendingRequest < 0
* Aggiunta proprietà jQuery.validator.autoCreateRanges, con valore predefinito false, per abilitare la conversione di min/max in range e di minlength/maxlength in rangelength; corregge principalmente il problema introdotto con la creazione automatica degli intervalli nella versione 1.2
* Corretto optional-methods in modo da non evidenziare nulla se il campo è vuoto, ovvero non attivare l'esito positivo
* Consentire false/null per le opzioni highlight/unhighlight anziché forzare do-nothing-callback anche quando non è necessario evidenziare nulla
* Corretta chiamata validate() senza elementi selezionati, restituendo un valore non definito anziché generare un errore
* Migliorata demo, tramite la sostituzione di metadati con classi/attributi per specificare regole
* Corretto errore quando non viene usato un messaggio personalizzato per la convalida remota
* Modificata convalida di posta elettronica e url per richiedere l'etichetta del dominio e l'etichetta principale
* Corretta convalida url e di posta elettronica per richiedere TLD (per richiedere effettivamente l'etichetta del dominio); spostata versione 1.2 (TLD facoltativo) in aggiunte come url2 e email2
* Corretta demo dynamic-totals in IE6/7 e migliorata creazione dei modelli, usando textarea per archiviare l'interpolazione di stringhe e modelli multiriga
* Aggiunto esempio di modello di accesso con collegamento "Password di posta elettronica" che rende facoltativo il campo della password
* Migliorata demo dynamic-totals con un esempio di un singolo messaggio per due campi

<a name="12"></a>1.2
---

* Aggiunto esempio di convalida captcha AJAX (basato su http://psyrens.com/captcha/)
* Aggiunta remember-the-milk-demo (grazie al team RTM per l'autorizzazione)
* Aggiunta marketo-demo (grazie a Glen Lipka)
* Aggiunto supporto per la convalida ajax, vedere metodo "remote"; sul lato server viene restituito JSON, true per elementi validi, false o una stringa per elementi non validi, stringa usata come messaggio
* Aggiunte opzioni highlight e unhighlight, per impostazione predefinita attiva o disattiva errorClass sull'elemento, consente l'evidenziazione personalizzata
* Aggiunto metodo del plug-in valid() per un facile controllo a livello di codice del modulo e dei campi senza la necessità di usare l'API del validator
* Aggiunto metodo del plug-in rules() per leggere e scrivere regole per un elemento (attualmente di sola lettura)
* Sostituito regex per il metodo email, grazie al contributo di Scott Gonzalez, vedere http://projects.scottsplayground.com/email_address_validation/
* Ristrutturata architettura di eventi in modo da basarsi solo sulla delega, migliorando le prestazioni e la facilità d'uso per gli sviluppatori (richiede jquery.delegate.js)
* Spostata documentazione da inline a http://docs.jquery.com/Plugins/Validation - inclusi esempi interattivi per tutti i metodi
* Rimosso validator.refresh(), la convalida ora è completamente dinamica
* Rinominati minValue in min, maxValue in max e rangeValue in range, deprecando i nomi precedenti (da rimuovere nella versione 1.3)
* Rinominati minLength in minlength, maxLength in maxlength e rangeLength in rangelength, deprecando i nomi precedenti (da rimuovere nella versione 1.3)
* Aggiunta funzionalità per unire min + max in range e minlength + maxlength in rangelength
* Aggiunto supporto per parametri delle regole dinamiche, consentendo di specificare una funzione come parametro, ad es. per minlength, chiamato durante la convalida dell'elemento
* Consente di specificare null o una stringa vuota come messaggio per non visualizzare nulla (vedere demo marketo)
* Miglioramento delle regole: Ora supporta la combinazione di opzione rules, metadati, classi (nuove) e attributi (nuovi), vedere Rules () per informazioni dettagliate

<a name="112"></a>1.1.2
---

* Sostituito regex per il metodo URL, grazie al contributo di Scott Gonzalez, vedere http://projects.scottsplayground.com/iri/
* Migliorato metodo di posta elettronica per gestire meglio i caratteri Unicode
* Corretto contenitore errori da nascondere quando tutti gli elementi sono validi, non solo per l'invio di moduli
* Corretto String.format in jQuery.format (con spostamento nello spazio dei nomi di jQuery)
* Corretto metodo accept per accettare le estensioni in maiuscolo e minuscolo
* Corretto metodo del plug-in validate() per creare solo un'istanza del validator per un determinato modulo e restituire sempre la stessa istanza (evitando di associare più volte gli eventi)
* Modificato log della console in modalità di debug dal livello "errore" ad "avviso"

<a name="111"></a>1.1.1
-----

* Corretto XHTML non valido, in modo da impedire la creazione di etichette di errore in IE a partire da jQuery 1.1.4
* Fisso e migliorato String. Format: Ricerca e sostituzione, gestione più efficace degli argomenti di matrice globale
* Corretta gestione del pulsante Annulla per usare l'oggetto validator per l'archiviazione dello stato anziché l'elemento form
* Corretti selettori dei nomi per gestire nomi "complessi", ad es. contenenti parentesi quadre ("list[]")
* Aggiunto pulsante e disabilitati elementi da escludere dalla convalida
* Spostati gestori eventi degli elementi per aggiornare la possibilità di aggiungere gestori a nuovi elementi
* Corretta convalida di posta elettronica per consentire domini di livello superiore lunghi (ad es. ".travel")
* Spostato showErrors() da valid() a form()
* Aggiunto validator.size(): restituisce il numero di errori correnti
* Chiamata di submitHandler con validator come ambito per semplificare l'accesso ai relativi metodi, ad es. per trovare le etichette di errore usando errorsFor(Element)
* Compatibile con jQuery 1.1.x e 1.2.x

<a name="11"></a>1.1
---

* Aggiunta della convalida su blur, keyup e click (per caselle di controllo e pulsanti di opzione). Sostituisce l'opzione event.
* Corretto resetForm
* Corretta custom-methods-demo

<a name="10"></a>1.0
---

* Migliorati metodi number e numberDE per verificare i numeri decimali corretti con delimitatori
* Vengono controllati solo gli elementi che dispongono di regole (in caso contrario l'opzione esito positivo è applicata a tutti gli elementi)
* Aggiunto metodo numero di carta di credito (grazie a Brian Klug)
* Aggiunta opzione Ignora, ad es. ignore: "[@type=hidden]", usando questa espressione per escludere gli elementi da convalidare. Impostazione predefinita: none, anche se i pulsanti Invia e Reimposta vengono sempre ignorati
* Notevolmente migliorate funzioni come messaggi fornendo un helper String.format flessibile
* Accettate funzioni come messaggi, per fornire messaggi personalizzati di runtime
* Corretta esclusione di elementi senza regole da successList
* Corretta custom-method-demo, sostituito avviso con un messaggio che visualizza il numero di errori
* Corretto form-submit-prevention quando si usa submitHandler
* Completamente rimossa la dipendenza dagli ID elemento, anche se sono ancora usati (quando presenti) per collegare le etichette di errore agli input. Ottenuta usando una matrice con {name, message, element} anziché un oggetto con coppie id:message per l'elenco errorList interno.
* Aggiunto supporto per specificare regole semplici come stringhe semplici, ad es. "required" equivale a {required: true}
* Aggiunta funzionalità: Aggiungere errorClass all'elemento padre valido campo s, rendendo più semplice definire lo stile contenitore/campo dell'etichetta o etichetta per il campo.
* Aggiunta funzionalità: focusCleanup - se abilitata, rimuove errorClass dagli elementi non validi e nasconde tutti i messaggi di errore ogni volta che l'elemento è con stato attivo.
* Aggiunta opzione esito positivo per visualizzare che un campo è stato convalidato correttamente
* Corretto problema di selezione di Opera (per evitare un conflitto di attributi)
* Corretti problemi di stato attivo degli elementi nascosti in IE
* Aggiunta funzionalità per ignorare la convalida per i pulsanti Invia con la classe "cancel"
* Corretti potenziali problemi con la barra degli strumenti di Google preferendo i messaggi di opzione del plug-in all'attributo title
* submitHandler viene chiamato solo quando un evento submit effettivo è stato gestito, validator.form() restituisce false solo per i moduli non validi
* Gli elementi non validi ora sono con stato attivo solo all'invio o tramite validator.focusInvalid(), evitando problemi con focus-on-blur
* Il problema di layout del contenitore errori di IE6 è stato risolto
* Personalizzazione elemento error tramite l'opzione errorElement
* Aggiunto validator.refresh() per trovare nuovi input nel modulo
* Aggiunto metodo di convalida accept, controlla le estensioni dei file
* Migliorata funzionalità di dipendenza con l'aggiunta di due espressioni personalizzate: ":blank" per selezionare elementi con un valore vuoto e ":filled" per selezionare elementi con un valore, in entrambi i casi escludendo gli spazi vuoti
* Aggiungere un metodo resetform () al validator: Reimposta ogni elemento del form (mediante il plug-in di modulo, se disponibile), rimuove le classi per gli elementi non validi e nasconde tutti i messaggi di errore
* Corretti documenti per validator.showErrors()
* Corretta creazione di etichette di errore in modo da usare sempre html() anziché text(), consentendo il passaggio di HTML arbitrario come messaggi
* Corretta creazione di etichette di errore in modo da usare la classe di errore specificata
* Aggiunta funzionalità di dipendenza: Il metodo richiesto accetta sia String (espressioni jQuery) che funzioni come argomento
* Notevolmente migliorata personalizzazione del messaggio di errore: Usare i messaggi normali e mostrare/nascondere un contenitore aggiuntivo; Sostituire completamente visualizzazione del messaggio con un meccanismo personalizzato (pur essendo in grado di delegare al gestore predefinito; Personalizzazione posizionamento delle etichette generate (anziché sotto-elemento predefinito)
* Corretti due bug principali in IE (contenitori errori) e Opera (metadati)
* Modificati metodi di convalida per accettare i campi vuoti come validi (eccezione: naturalmente metodi "required" e "equalTo")
* Rinominati "min" in "minLength", "max" in "maxLength", "length" in "rangeLength"
* Aggiunti "minValue", "maxValue" e "rangeValue"
* Semplificata API per il supporto di diversi eventi. Il valore predefinito, submit, può essere disabilitato. Se viene specificato un evento che viene applicato a ogni elemento (anziché all'intero modulo). La combinazione di keyup-validation con submit-validation è ora estremamente semplice da configurare
* Aggiunto supporto per un messaggio per regola quando si definiscono i messaggi tramite le impostazioni del plug-in
* Aggiunto supporto per eseguire il wrapping dei metadati in un elemento padre. Utile anche quando i metadati vengono usati per altri plug-in.
* Refactoring di test e demo: Meno file, demo migliori
* Documentazione migliorata: Altri esempi per i metodi, più testi spiegare alcuni concetti di base di riferimento
