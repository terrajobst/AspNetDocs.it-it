---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Uso di SSL nell'API Web | Microsoft Docs
author: MikeWasson
description: Viene illustrato come utilizzare SSL con API Web ASP.NET, incluso l'utilizzo di certificati client SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598637"
---
# <a name="working-with-ssl-in-web-api"></a>Working with SSL in Web API (in inglese)

di [Mike Wasson](https://github.com/MikeWasson)

Diversi schemi di autenticazione comuni non sono sicuri su HTTP normale. In particolare, l'autenticazione di base e l'autenticazione basata su form inviano credenziali non crittografate. Per garantire la sicurezza, questi schemi di autenticazione *devono* usare SSL. Inoltre, è possibile usare i certificati client SSL per autenticare i client.

## <a name="enabling-ssl-on-the-server"></a>Abilitazione di SSL nel server

Per configurare SSL in IIS 7 o versione successiva:

- Creare o ottenere un certificato. Per il test, è possibile creare un certificato autofirmato.
- Aggiungere un'associazione HTTPS.

Per informazioni dettagliate, vedere [come configurare SSL in IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

Per il test locale, è possibile abilitare SSL in IIS Express da Visual Studio. Nella Finestra Proprietà impostare **SSL abilitato** su **true**. Prendere nota del valore dell' **URL SSL**. utilizzare questo URL per testare le connessioni HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Applicazione di SSL in un controller API Web

Se si dispone di un'associazione HTTPS e HTTP, i client possono comunque utilizzare HTTP per accedere al sito. È possibile consentire che alcune risorse siano disponibili tramite HTTP, mentre altre risorse richiedono SSL. In tal caso, usare un filtro azioni per richiedere SSL per le risorse protette. Il codice seguente illustra un filtro di autenticazione di API Web che verifica la presenza di SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Aggiungere questo filtro a tutte le azioni di API Web che richiedono SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certificati client SSL

SSL fornisce l'autenticazione tramite certificati di infrastruttura a chiave pubblica. Il server deve fornire un certificato che autentica il server per il client. È meno comune per il client fornire un certificato al server, ma questa è un'opzione per l'autenticazione dei client. Per usare i certificati client con SSL, è necessario un modo per distribuire i certificati firmati agli utenti. Per molti tipi di applicazioni, questa non sarà un'esperienza utente corretta, ma in alcuni ambienti (ad esempio, Enterprise) può essere fattibile.

| Vantaggi | Svantaggi |
| --- | --- |
| -Le credenziali del certificato sono più avanzate di nome utente/password. -SSL fornisce un canale sicuro completo, con autenticazione, integrità dei messaggi e crittografia dei messaggi. | -È necessario ottenere e gestire i certificati PKI. -La piattaforma client deve supportare i certificati client SSL. |

Per configurare IIS in modo che accetti i certificati client, aprire Gestione IIS e seguire questa procedura:

1. Fare clic sul nodo sito nella visualizzazione albero.
2. Fare doppio clic sulla funzionalità **Impostazioni SSL** nel riquadro centrale.
3. In **certificati client**selezionare una di queste opzioni: 

    - **Accept**: IIS accetterà un certificato dal client, ma non ne richieda uno.
    - **Richiedi**: richiedere un certificato client. (Per abilitare questa opzione, è necessario selezionare anche "Richiedi SSL")

È anche possibile impostare queste opzioni nel file ApplicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Il flag **SslNegotiateCert** indica che IIS accetterà un certificato dal client, ma non ne richiede uno (equivalente all'opzione "Accept" in Gestione IIS). Per richiedere un certificato, impostare il flag **SslRequireCert** . Per il test, è anche possibile impostare queste opzioni in IIS Express nel file ApplicationHost locale. File di configurazione, situato in "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Creazione di un certificato client per il test

A scopo di test, è possibile utilizzare [Makecert. exe](/windows/desktop/SecCrypto/makecert) per creare un certificato client. Prima di tutto, creare un'autorità radice di test:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert chiederà di immettere una password per la chiave privata.

Aggiungere quindi il certificato all'archivio "autorità di certificazione radice attendibili" del server di test, come indicato di seguito:

1. Aprire MMC.
2. In **file**selezionare **Aggiungi/Rimuovi snap-in**.
3. Selezionare **account computer**.
4. Selezionare **computer locale** e completare la procedura guidata.
5. Nel riquadro di spostamento espandere il nodo "autorità di certificazione radice attendibili".
6. Scegliere **tutte le attività**dal menu **azione** , quindi fare clic su **Importa** per avviare l'importazione guidata certificati.
7. Individuare il file del certificato, TempCA. cer.
8. Fare clic su **Apri**, quindi su **Avanti** e completare la procedura guidata. (Verrà richiesto di immettere nuovamente la password).

A questo punto creare un certificato client firmato dal primo certificato:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Uso dei certificati client nell'API Web

Sul lato server, è possibile ottenere il certificato client chiamando [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) nel messaggio di richiesta. Il metodo restituisce null se non è presente alcun certificato client. In caso contrario, restituisce un'istanza di **X509Certificate2** . Utilizzare questo oggetto per ottenere informazioni dal certificato, ad esempio l'autorità emittente e il soggetto. È quindi possibile usare queste informazioni per l'autenticazione e/o l'autorizzazione.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
