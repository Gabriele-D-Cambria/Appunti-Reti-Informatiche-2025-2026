---
title: Server APACHE
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. HTTP](#2-http)
- [3. APACHE HTTP server](#3-apache-http-server)
	- [3.1. Configurazione](#31-configurazione)
	- [3.2. Direttive Globali](#32-direttive-globali)
	- [3.3. Direttive per i siti web - Virtual Host](#33-direttive-per-i-siti-web---virtual-host)
	- [3.4. Moduli Utili](#34-moduli-utili)
- [4. Multi-Processing Module](#4-multi-processing-module)
	- [4.1. MPM prefork](#41-mpm-prefork)
	- [4.2. MPM Worker](#42-mpm-worker)
	- [4.3. MPM Event](#43-mpm-event)

# 2. HTTP

L'_HyperText Transfer Protocol_ è un protocollo di livello applicazione er lo scambio di _ipermedia_.

Segue l'architettura **client/server**, dove il _client_ (_browser_) chiede un documento e il server `HTTP` risponde con la risorsa.

È un protocollo _stateless_, dove il server non tiene traccia delle precedenti connessioni, e ogni scambio di richiesta/risposta è indipendente dai precedenti.

Nella prima versione `1.0 (1996)` le comunicazioni erano **non presistenti**, ma già un anno dopo `HTTP 1.1` prevedeva comunicazioni **persistenti** per le risorse,

Con l'aumento dei dispositivi monbili e dell'utilizzo di contenuti multimediali e della richiesta di esperienze web più veloci ed efficienti, si è implementata una nuova versione del protocollo `HTTP 2` che ha introdotto una serie di nuove tecnologie, come **Server Push**, **Multiplexing**, **Priorità del flusso**, **Compressione header**, **Protocollo binario**, ...

Nel 2022 l'evoluzione in `HTTP 3` ha mirato a migliorare l'efficienza, passando da una semplice richiesta `GET` ad un protocollo `QUIC`, un protocollo avanzato basato su `UDP` con multiplexing e crittografia integrata. `QUIC` riduce la latenza e aumenta la sicurezza.

Vediamo degli esempi di messaggi `HTTP`:
<div class="grid2">
<div class="top">
<div class="p">Richiesta</div>

```log
:method: GET
:scheme: https
:authority: www.example.com
:path: /index.html
user-agent: curl/7.79.1
accept: */*
```
</div>
<div class="top">
<div class="p">Risposta</div>

```log
HTTP/3 200 OK
Date: Wed, 26 Jan 2024 12:34:56 GMT
Server: Apache/2.4.46 (Unix)
Last-Modified: Mon, 10 Jan 2024 10:15:20 GMT
Content-Length: 438
Content-Type: text/html; charset=UTF-8
<qui ci sono i dati>
```
</div>
</div>


# 3. APACHE HTTP server

Apache è un server che permette di hostare server web, implementato tramite `apache2`.

```bash
sudo apt update
sudo apt upgrade
sudo apt install apache2
```

Dopo l'installazione dovrebbe avviarsi automaticamente, ma è comunque possibile verificare tramite:
```bash
systemctl status apache2
```

È possibile verificare anche `http://localhost` in un qualsiasi browser sulla stessa macchina.
In questo caso verrà mostrata la pagina che si trova in `/var/www/html/index.html`.

Per interagire con un server `APCHE` è possibile:
```bash
sudo apache2ctl <comando> # start, stop, restart, status, configtest

# opuure #
sudo service apache2 <comando> # start, stop, restart, reload
```

## 3.1. Configurazione 

La directory principale si trova in  `/etc/apache2`.
Al suo interno il file di configurazione principale è `/etc/apache2/apache2.conf`.

La configurazione si specifica attraverso delle direttive, eventualmente raggruppate da **direttive contenutore**:
```conf
# Commento descrittivo
Direttiva1 valore
Direttiva2 valore

<Contenutore valore>
	Direttiva3 valore
	Direttiva4 valore
</Contenutore> # Fine contenutore
```

In generale si trovano altre configurazioni aggiuntive in:
- `/etc/apache2/conf-available`: contiene pezzi di configurazione non attive
- `/etc/apache2/conf-enabled`: contiene link a configurazioni aggiuntive che sono attive
- `/etc/apache2/mods-available`: contiene link a moduli non attivi, ovvero configurazioni complesse che specificano come il server deve gestire le richieste da più client
- `/etc/apache2/mods-enabled`: contiene link a moduli attivi, configurazioni complesse che specificano come il server deve gestire le richieste da più client
- `/etc/apache2/ports.conf`: descrive quali porte sono disponibili
- `/etc/apache2/sites-available`: contiene link a file di configurazioni specifici a siti non però abilitati
- `/etc/apache2/sites-enabled`: contiene link a file di configurazioni specifici a siti abilitati

Il sistema modulare funziona così:
```conf
# File apache2.conf
...
# Includo la lista di porta sulle quali stare in ascolto
Include ports.conf

# ----------------------- #
# File ports.conf

Listen 80
```

Per (dis)abilitare un file si utilizza il comando:
```bash
a2enconf <nome_file> # abilito

a2disconf <nome_file> # disabilito
```

Questo comando crea un _soft link_ nella directory `/etc/apache2/conf-enabled`, file he viene incluso di default all'interno di `apache2.conf`.

Dopo una qualsiasi modifica **è necessario riavviare il server per ricaricare la configurazione**.

Per i moduli si (dis)abilitano:
```bash
a2enmod <nome_file> # abilito
a2dismod <nome_file> # disabilito
```

Se due moduli adassero in conflitto, il server ritorna un messaggio di errore durante il caricamento della nuova configurazione.

## 3.2. Direttive Globali

Sono configurazioni generali che vengono applicate al server Apache nel suo complesso, indipendentemente dai siti web o dai _virtual host_ ospitati.
Sono impostate nel file `apache2.conf` e possono riguardare:
- impostazioni di sicurezza
- porte su cui Apache ascolta
- directory predefinite
- impostazioni di logging

A meno che non vengano sovrascritte a livello di _virtual host_ o _dorectory_, **_vengono applicate a tutte le richieste e a tutte le risorse servite dal server_**.

La prima direttiva che vediamo è la **_server root_**.
Di default la root del server è:
```conf
ServerRoot /etc/apache2
```

Questa specifica la directory principale contenente i file di configurazione di Apache, e rappresenta la base sulla quale sono risolti tutti i path relativi.

Questa direttiva è **_configurata automaticamente all'avvio di `apache2ctl`_**, infatti nel file `apache2.conf` è commentata.

Altre direttive sono:
```conf
KeepAlive on 			# se offrire o meno le connessioni persistenti HTTP 1.1
KeepAliveTimeout 5		# quanti secondi attendere la richiesta successiva dal client prima di chiudere la connessione
MaxKeepAliveRequest 100	# qauante richieste può fare al massimo un client sulla stessa connessione prima di chiuderla
```

Queste direttive sono ad oggi deprecate, ma comunque presenti nel file quindi è bene sapere cosa fanno.
In particolare un valore troppo alto per la `KeepAliveTimeout` potrebbe bloccare inutilmente un processo che sta servendo un _client_ lento o disconnesso.

Un altra direttiva importante è la seguente:
```conf
Listen 80
Listen 8080
```

Questa direttiva obbligatoria, presente in `/etc/apache2/ports.conf` che viene incluso in `apache2.conf`, permette di specificare le porte che userà `Apache` per mettersi in ascolto di connessioni.

Per recuperare gli errori è possibile utilizzare un file di log, specificandolo:
```conf
ErrorLog /var/log/apache2/error.log		# directory di default
```

Il formato e la verbosità dei messaggi possono essere definiti con le altre direttive `ErrorLogFormat` e `LogLevel`.

## 3.3. Direttive per i siti web - Virtual Host

Nel caso più semplice, un server web è in esecuzione su **una macchina con un unico indirizzo IP offrendo un solo sito Web**.

In questo caso, per avere $n$ siti web saranno necessarie $n$ macchine, utilizzando quindi tantissime risorse.

COn il _**name-based Virtual Hosting**_ si possono configurare più siti **_sullo stesso server Web_**, sulla stessa macchina con lo stesso indirizzo IP.

Il server Web discrimina le richieste dei _client_ in base al campo `Host` della richiesta `HTTP`:
```log
GET /index.html HTTP/1.1
Host: www.mysite1.com
...
```

I _virtual host_ si mettono in `/etc/apache2/sites-available`:
```bash
a2ensite <nome_file>	# abilita un sito al riavvio del server
a2dissite <nome_file>	# disabilita un sito al riavvio del server
```

Per definire un _virtual host_ si utilizza la seguente sintassi:
```conf
<VirtualHost ip:porta>
...
</VirtualHost>
```

Nel caso più semlice Apache ha un _Virtual Host_ abilitato di default in `/etc/apache2/sites-available/000-default.conf`:
```conf
<VirtualHost *:80>
	#ServerName www.example.com				Nome simbolico del sito
	ServerAdmin webmaster@localhost
	...
	DocumentRoot /var/www/html
	ErrorLog ${APACHE_LOG_DIR}/error.log	# uguale alla direttiva globale
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Questa configurazione risponde a **_tutti gli IP sulla porta 80_**.

## 3.4. Moduli Utili

Il modulo `/etc/apache2/mods-availaable/dir.conf`, abilitato didefualt, permette di specificare attraverso la direttiva `DirectoryIndex` **_il file da prelevare in caso non venga specificato dal client_**.
```conf
DirectoryIndex index.html index.cgi index.pl index.php ...
```

In questo modo quando l'utente digita `www.mysite.com` riceverà il file `www.mysite.com/index.html`

Il modulo `/etc/apache2/mods-availaable/userdir.conf`, NON abilitato didefualt, permette invece ai singoli utenti di mettere i propri file nella cartella speciicfata e renderli accessibili tramite l'indirizzo `www.mysite.com/~username/`.
```conf
UserDir public_html
```

# 4. Multi-Processing Module

Affinché il server possa accettare e servire più richieste contemporaneamente Apache ricorre ad un _Multi-Processing Module_ `MPM`, responsabile di gestire i socket, fare binfig delle porte, accettare connessioni e servirle, eventualmente usande processi e thread figli.

Su `UNIX` si può scegliere tre:
- prefork
- worker
- event

## 4.1. MPM prefork

Implementa un **_server multiprocesso senza thread_**.
All'avvio, un processo padre lancia un certo numero di processi figli (_preforking_) che restano in ascolto accettando e servendo connessioni.
Dopo aver servito una connessione tornano disponibili per una nuova connessione.

Il padre gestisce il _pool_ dei figli, cercando di mantenere sempre alcuni figli disponibili.

Il preforking permette di diminuire l'overhead della `fork()` ad ogni connessione.

ogni figlio viene riciclato per `MaxConnectionsPerChild` connessioni, e poi viene ucciso per evitare _memory leak_ accidentali.
Il server genera all'avvio `StartServer n` figli. Il numero massimo di figli creabili dalle connessioni aggiuntive è `MaxRequestWorkers`.

I figli inattivi devono essere compresi nell'intervallo `[MinSpareServers; MaxSpareServers]`.

Tutte queste variabili si trovano in `/etc/apache2/mods-available/prefork.conf`

Questo approccio ha come vantaggio:
- Massima compatibilità: in alcuni casi alcuni moduli o librerie potrebbro non supportare il _multithreading_
- Massima stabilità: un processo che crasha interrompe una sola connessione

Ha però anche degli svantaggi:
- I processi occupano più memoria dei thread
- Bisogna regolare bene le impostazioni per minimizzare lo spazio e l'_overhead_.

## 4.2. MPM Worker

Permette di avere un **_server-multiprocesso e multi-thread_** riducendo l'_overhead_.

Il processo padre genera un certo numero di figli, e ogni figlio genera:
- Un _thread listener_ che accessa e smista le connessioni
- Un certo numero di _thread workers_ che servono le connessioni

Anche in questo caso il server genera `StartServer n` figli, che verranno riciclati per `MaxConnectionsPerChild` connessioni.

I figli generano all'inizio `ThreadsPerChild`, e il numero massimo di **thread totali** è `MaxRequestWorkers`, e anche in questo caso i _thread_ devono essere compresi tra `[MinSpareThreads, MaxSpareThreads]`,,

Il numero massimo di figli è **_necessariamente_** `MaxRequestWorkers/ThreadsPerChild`.

## 4.3. MPM Event

È una versione migliorata di `MPM worker`, ed è il modulo di defautl dalla versione `2.4`.

Oltre ad accettare le connessioni, il thread listener gestisce le connessioni **_temporaneamente inattive_**.
Ad esempio:
- Se un _worker_ è connesso a un _client_ che sta tardando a inviare una richiesta, invece di attendere, **restituisce il controllo del socket al listener** e passa a servire un altro client.
  Quando il primo client invierà la richiesta, il listener la assegnerà ad un altro worker libero.
- Se un _worker_ sta servendo un _client_ con una connessione lenta e il buffer di invio del socket si riempie. Invece di attendere, restituisce il controllo del socket al listener che lo assegnerà ad un altro worker non appena sarà di nuovo scrivibile.

In questo modo aumentiamo il numero di connessioni servibili contemporaneamente a parità di numero di thread, eliminando i tempi morti.

Alcuni limiti globali sono:
- `ThreadLimit`: limite massimo configurabile per il numero di _thread attivi per processo_.
  - `Worker(thread)`: `ThreadsPerChild <= ThreadLimit`
- `ServerLimit`: limite massimo configurabile per il nuemro di **processi figli attivi**.
  - `Prefork`: `MaxRequestWorkers <= ServerLimit`
  - `Worker(thread)`: `MaxRequestWorkers <= ServerLimit * ThreadsPerChild`