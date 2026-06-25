---
title: Reti Wireless e Nodi Mobili
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Reti Wireless e Nodi Mobili](#2-reti-wireless-e-nodi-mobili)
- [3. Rete Wireless](#3-rete-wireless)
  - [3.1. Tipi di Infrastrutture](#31-tipi-di-infrastrutture)
  - [3.2. Caratteristiche dei Link e della Rete](#32-caratteristiche-dei-link-e-della-rete)
  - [3.3. Reti Wireless `LAN` - `WiFi`](#33-reti-wireless-lan---wifi)
    - [3.3.1. Processo di Collision Avoidance - `CSMA-CA`](#331-processo-di-collision-avoidance---csma-ca)
    - [3.3.2. Addressing](#332-addressing)
    - [3.3.3. Mobilità a Subnet Unica](#333-mobilità-a-subnet-unica)
    - [3.3.4. Capacità Avanzate](#334-capacità-avanzate)
  - [3.4. Reti Wireless `LAN` - `LiFi`](#34-reti-wireless-lan---lifi)
  - [3.5. Reti Wireless `PAN` - `Bluetooth`](#35-reti-wireless-pan---bluetooth)
  - [3.6. Reti Cellulari `WAN` - `4G` e `5G`](#36-reti-cellulari-wan---4g-e-5g)
- [4. Nodi Mobili](#4-nodi-mobili)
  - [4.1. Routing Indiretto](#41-routing-indiretto)
  - [4.2. Routing Diretto](#42-routing-diretto)
  - [4.3. Impatto della mobilità sui livelli superiori](#43-impatto-della-mobilità-sui-livelli-superiori)

# 2. Reti Wireless e Nodi Mobili

Oggi la maggior parte dei dispositivi _client_ connessi ad internet sono dispositivi **mobili** o _wireless_. Nel 2019 il numero di dispositivi connessi attraverso telefonia mobile era infatti in rapporto 10 a 1 rispetto alla telefonia fissa, ed il rapporto è in costante aumento.

È quindi necessario affrontare due nuove sfide:
- Comunicazioni Wireless
- Nodi mobili che cambiano il punto di aggancio alla rete

# 3. Rete Wireless

<div class="grid2">
<div class="">

Nella figura sulla destra possiamo vedere il contesto nel quale esploreremo l'argomento delle reti wireless.

Identifichiamo innanzitutto gli elementi di una rete wireless:
- **Wireless Hosts**: sono i sistemi finali che eseguono le applicazioni. Fanno parte degli _host_ tutti i dispositivi in grado di connettersi alla rete wireless, indipendentemente dal fatto che siano mobili o meno.
- **Wireless Access Point** (`AP`): è un dispositivo di _bridge_, detto anche _base station_, con due interfacce:
  - La prima è connessa in maniera _linked_ con la rete internet cablata
  - La seconda è connessa in maniera _wireless_ ai dispositivi
- **Wireless Link**: sono i _link_ utilizzati per dagli host per connettersi alle _base stations_ o ad altri host wireless.

L'insieme di più _wireless hosts_ e di un `AP` è indicata come **_basic-service-set_** (`BSS`).

Chiamiamo **_handoff_** quando un _host_ si sposta oltre la portata di una _base station_, mentre chiamiamo **_handover_** quando entra nel range di un'altra, cambiando il proprio _access point_ in relazione alla rete internet.

Questi processi introducono la necessità gestire opportunamente la transizione di un dispositivo durante lo spostamento da una cella ad un altra. Per fare ciò, dobbiamo capire come fare a:
- Localizzare un dato _host_ mobile all'interno della rete in un certo istante
- Effettuare correttamente l'indirizzamento dei dati, dato che potrebbero dover essere reindirizzati in nuovi _access point_ senza interrompere la connessione.

</div>
<div class="">
<img class="75" src="./images/wireless/infrastucture-scheme.png">
</div>
</div>

La tecnologia sulla quale un _wireless link_ è basato ne influenza la _portata_ e il _bit-rate_.

<div class="grid2">
<div class="">

La tecnologia con _range_ e _bit-rate_ più basso è la **_Bluetooth_** (`802.15` stadard per reti short-range-low-power, la versione `802.15.1` indica la versione _high-bit-rate_ ovvero `1Mbps`), che è stata pensata per connettere diversi dispositivi sulle scrivanie.

Successivamente troviamo le reti `WiFi`, che è un nome commerciale di una tecnologia basata sul protocollo `IEEE 802.11`.
Hanno area di copertura locale ($\approx 100m$) e permettono trasmissioni con _bit-rate_ che variano a seconda delle vare versioni (dagli `11Mbps` fino alla velocità nominale di `14Gbps`).

Andando ad aumentare il raggio di copertura troviamo:
- Tecnologie _long-range_ di `WiFi` (ad esempio per la lettura dei contatori)
- Reti Mobili, come `4G LTE` e `5G`. La differenza sostanziale tra di loro è che `5G` permette un _bit-rate_ più elevato a discapito di un range minore.

</div>
<div class="">
<img class="" src="./images/wireless/link-characteristics.png">
</div>
</div>


Siamo abituati a vedere le reti wireless basate su un infrastruttura che permette di comunicare con la rete.
Quando un dispositivio opera nella portata di una _base station_ si dice infatti che questo opera nella _**infrastucture mode**_.
Tuttavia è possibile utilizzare la tecnologia wireless in modalità _**ad-hoc**_ che permette la comunicazione tra più dispositivi in maniera wireless senza l'ausilio di alcuna infrastruttura (bluetooth, quick share, air drop, ...).
In questo tipo di tecnologia sono gli _host_ stessi a fornire i servizi di routing, assegnamento degli indirizzi, traduzioni simil-DNS, ...

## 3.1. Tipi di Infrastrutture

Identificando l'**_infrastruttura di rete_** come la rete sulla quale un _host_ wireless potrebbe voler comunicare, vediamo adesso come i vari "pezzi" di una rete wireless possono essere assemblati per ottenere diversi tipi di reti wireless.

In particolare, ci basiamo adesso su due criteri:
1. Il numero di _hops_ effettuati da un pacchetto nella rete wireless
2. La presenza o meno delle _base stations_

Le principali categorie sono quindi identificabili con la seguente tabella:

<div class="flexbox" markdown="1">

|                                   |                                                                          _**Single Hop**_                                                                           |                                                                                                                                                                                                                  _**Multiple Hops**_                                                                                                                                                                                                                   |
| :-------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|    _**Infrastructure-based**_     |    È presente una _base-station_ connessa ad una rete più ampia.<br><br>Un esempio sono le reti `IEEE 802.11` (_WiFi_) e le reti cellulari come `4G LTE` o `5G`.    |                                                                                                   È presente una _base-stvation_ connessa ad una rete più ampia, ma è consentita la<br>comunicazione tra nodi wireless per raggiungere gli _host_ più distanti.<br><br>Un esempio sono le **wireless mesh networks** e le **wireless sensors networks**                                                                                                    |
| _**Ad-hoc, Infrastructure-less**_ | Non è presente alcuna _base-station_, ma sono i singoli<br>nodi wireless a coordinare le trasmissioni da e verso i nodi.<br><br>Un esempio sono le **reti Bluetooth**. | Non è presente alcuna _base-station_, ma sono i singoli<br>nodi wireless a coordinare le trasmissioni da e verso i nodi.<br>In questo caso i nodi possono essere mobili, cambiando la loro connettività verso gli altri.<br><br>Un esempio sono i **Mobile Ad-hoc NETworks** (`MANETs`) o i **Vehicular Ad-hoc NETwork** (`VANET`)<br>nel caso in cui i nodi mobili fossero dei veicoli.<br><br>Questo tipo di infrastruttura è ancora oggi in forte sviluppo. |

</div>

## 3.2. Caratteristiche dei Link e della Rete

Le principali differenze dei link wireless rispetto ai link fisici sono:
- **Minore forze del segnale**: il segnale wireless è trasmetto sfruttando onde elettromagnetiche, che sia ttenuano notevolmente quando sono propagati attraverso la materia. Anche se propagato nel vuoto il segnale si disperde molto più velocemente rispetto ai segnali fisici. Il fenomeno è chiamato _path loss_.
- **Maggior esposizione a interferenze**: i segnali wireless utilizzano la frequenza `2.4 GHz` che è condivisa tra più dispositivi, ad esempio anche dalla `802.11b wireless LAN`. Oggi la maggior parte dei protocolli `802.11` opera nella frequenza dei `5GHz`.
- **Propagazione Multipath**: il segnale radio si riflette sulle superfici, arrivando ad una destinazione da punti diversi in momenti leggermente diversi

Questi problemi sono alla base della maggiore difficoltà di effettuare comunicazioni _wireless_ rispetto alle _wired_.
Per questo motivo i protocolli wireless utilizzano potenti algoritmi di `CRC` ma supportano anche un protocollo di _link-level reliable-data-transfer_ che ritrasmette i frame corrotti.

Avendo in considerazione gli impedimenti dovuti al canale wireless, vediamo cosa succede all'_host_ che riceve il segnale wireless.
Quello che riceve è un mix tra una versione degradata del segnale originario e del _background noise_ dovuto all'ambiente.

<div class="grid2">
<div class="">



Si introduce quindi il **_Signal-to-Noise Ratio_** `SNR` che misura la forza relativa del segnale rispetto al _noise_:
$$
  \text{SNR} = 20 * \log_{10}{\text{Ampiezza}_\text{Segnale} \over \text{Ampiezza}_\text{noise}}
$$

In generale è possibile aumentare `SNR` aumentando la potenza fornita, questo però ha diversi _tradeoff_, quali il maggior utilizzo di energia e la maggior probabilità di interferenza rispetto ad altre comunicazioni.

Inoltre il fatto che i dispositivi sono mobili, può influenzare direttamente il `SNR`. Sarebbe quindi ottimale avere un modo per modulare la potenza del segnale a seconda delle condizioni nelle quali ci troviamo.

</div>
<div class="">
<figure class="80">
<img class="80" src="./images/wireless/BER-SNR.png">
<figcaption>

Questo grafico mette a confronto diverse codifiche dei segnali in relazione al loro `SNR` e `BER` (_Bit-Error-Rate_).

In generale vige la regola che _"dato uno schema di modulazione, più alto è `SNR`, più basso sarà `BER`"_
</figcaption>
</figure>
</div>
</div>

Sono inoltre introdotti due nuovi problemi. Per esporli immaginiamo di avere **3 nodi** (`A`, `B` e `C`) che comunicano tra di loro tramite rete wireless.

<div class="grid2">
<div class="">

Il primo è il **problema del Nodo nascosto**. Immaginiamo di avere un ostacolo tra `A` e `C` (ad esempio un muro schermante). In questo contesto `B` riesce a comunicare con entrambi, mentre `A` e `C` non si "vedono".
Può quindi accadere che sia `A` che `C` comunichino in contemporanea con `B` che riceve due segnali sovrapposti.
Questa collisione non è rilevata né da `A` che da `C`.

Un secondo problema, simile al primo, è quello dell'**Attenuazione del Segnale**.
Immaginiamo che `B` abbia nel raggio di trasmissione sia `A` che `C`, mentre `A` si trova fuori dal _range_ di `C` (e viceversa).
Quando `A` proverà a comunicare con `B`, se `C` lo sta già facendo, percepirà solamente del _rumore di fondo_ assumendo il canale libero e iniziando anch'egli a comunicare.
Anche in questo caso si ha una collissione in `B` che non è rilevata né da `A` che da `C`.

</div>
<div class="">
<img class="70" src="./images/wireless/wireless-collision-problems.png">
</div>
</div>

## 3.3. Reti Wireless `LAN` - `WiFi`

Le reti _WiFi_ è una famiglia di reti locali.

Il nome _WiFi_ è il nome commerciale, il nome ufficale è `IEEE 802.11 wireless LAN`.

<div class="grid2">
<div class="">

Nella tabella di fianco possiamo vedere l'evoluzione della tecnologia negli anni.

La tecnologia `802.11.ah` è utilizzata per i dispositivi che trasmettono a bit-rate elevati per distanze elevate, utile nel caso della gestione dei dispositivi IoT in ambienti ampi.

Tutte queste tecnologie utilizzano `CSMA/CA` per consentire l'accesso multiplo.
La ragione per il quale si utilizza `CSMA/CA` e non `CSMA/CD` è perché **non è sempre posibile per il trasmettitore rilevare le collisioni** che si verificano nella maggior parte dei casi nel ricevitore.

Inoltre tutte esistono sia in versione _ad-hoc_ che _infrastructure-based_.
</div>
<div class="">
<img class="70" src="./images/wireless/wireless-LAN-history.png">
</div>
</div>

Nell'architettura _WiFi_ un _host_ comunica con l'`Access Point` in un **Basic Service Set** (`BSS`), chiamato impropriamente anche _cell_ o _hotspot_.
Quando un _host_ entra in un `BSS` deve **_associarsi_**, ovvero selezionare una frequenza di comunicazione. In totale sono disponibili $11$ frequenze, per far sì che due `AC` vicini non facciano interferenza l'un l'altro utilizzando la stessa frequenza.

Quando un amministratore installa un `AP` dovrà scegliere, oltre ad un `SSID` da assegnare al dispositivo, una di queste frequenze. Da quel momento in poi l'`AP` procederà ad inviare un _beacon frame_ ogni $x$ millisecondi. All'interno di questo frame sono conservate le informazioni `<AP SSID, MAC address>`.

A questo punto un _host_, nel momento in cui si vuole connettere alla rete, procederà ad itererre sulle varie frequenze finché non rileva uno o più _beacon_, scegliendone uno secondo una determinata politica (tipicamente quello a potenza maggiore, ma non vi è alcuno standard).
Scelto un _beacon_, l'_host_ procederà quindi a recuperare le informazioni contenute nel suo messaggio per poi collegarsi con l'`AP` sorgente (eventualmente autenticandosi se necessario).

Una volta collegato, procederà come nelle reti cablate ad acquisire un indirizzo `IP` tramite `DHCP` per ottenere un indirizzo nella `AP subnet`.

### 3.3.1. Processo di Collision Avoidance - `CSMA-CA`

Analizziamo adesso come fuziona il processo di _Collision Avoidance_, basato su `CSMA/CA`.

Quando un _sender_ deve inviare un _frame_, attende controlla che il canale sia libero (_idle_) per un certo tempo pari a `DIFS` (_Distributed Inter-Frame Space_), e successivamente **_trasmette l'intero frame_**, senza effettuare _collision detection_.

Se invece trovasse il canale occupato, il _sender_ calcola un tempo casuale aggiuntivo $t_b$, detto di _backoff_ e procede ad attendere che il canale torni _idle_. Quando questo accade, il dispositivo attenderà adesso `DIFS` e poi per il tempo di backoff $t_B$:
- Per ogni slot che il canale è libero diminuirà $t_B$ di uno
- Per ogni slot che il canale è occupato manterrà inalterato il valore di $t_B$

Solamente quando $t_B = 0$ e il canale sarà libero, procederà ad inviare il messaggio.

L'intervallo $t_B$ viene scelto utilizzando scegliendo un numero casuale di slot da attendere. Questo numero è estratto uniformemente dall'intervallo $[0, CW-1]$, dove $CW$ rappresenta la **_Contention Window_**.

Inizialmente $CW$ assume un valore statico detto $CW_{\text{min}}$ (nello standard `802.11` corrisponde a $31$ slot), e ogni volta che non si riceve un `ACK` il valore di $CW$ **raddoppia**, fino a raggiungere un valore massimo detto $CW_{\text{max}}$ (tipicamente $1023$ slot).

Nel momento in cui un _frame_ viene spedito correttamente, il ricevitore ha il compito di inviare un messaggio di **ACK**. Questo messaggio non è inviato immediatamente, ma viene inviato dopo un intervallo detto `SIFS` (_Short Inter-Frame Space_), che deve essere _**strettamente minore**_ di `DIFS`, così da avere priorità sugli altri invii che potrebbero essere in attesa su gli altri dispositivi.

<img class="50" src="./images/wireless/CSMA-CA-sequence-diagram.png">

Poiché, come abbiamo già detto, può accadere che gli altri dispositivi non siano in grado di percepire che il canale è utilizzato, due pacchetti potrebbero andare in collisione. In questi casi, il dispositivo che riceve questi due pacchetti **non invierà** l'`ACK`.

Superato un tempo `SIFS` dalla fine della trasmissione del pacchetto, senza che l'`ACK` sia stato ricevuto, il _sender_ procederà con il reinvio, così da evitare un'ulteriore collisione sulla ritrasmissione. Se una stazione non riceve l'`ACK` per un numero fissato di volte, procedde a droppare il _frame_ e smette di ritrasmetterlo.

Di seguito un esempio più complesso nel quale vediamo cosa succede quando si prova a inviare un pacchetto con il mezzo occupato.

<img class="50" src="./images/wireless/CSMA-CA-sequence-diagram-backoff.png">

Questo processo diminuisce la probabilità di avere collisioni ma non la azzera. Infatti abbiamo ancora il problema che:
- I tempi scelti casualmente da due _host_, e quindi i loro _backoff_, possano essere uguali. Questa possibilità ha tuttavia una probabilità bassa, e per i nostri fini è quindi trascurabile.
- Il problema del nodo nascosto: seppur in realtà il meccanismo degli `ACK` risolva il probema, ci poniamo l'obiettivo di minimizzare i "tempi morti", che in questa casistica possono essere elevati, dato che la trasmissione dei _frame_ avviene per intero, anche dopo aver rilevato una collisione.

<div class="grid2">
<div class="">

Per riuscire a alleviare il problema del nodo nascosto si utilizza il **Virtual Carrier Sensing**, che mette i disposibivi in un "ascolto virtuale" del mezzo.

Quando un nodo `A` vuole trasmettere un messaggio `DATA` con il nodo `B`, invia innanzitutto una piccola **_Request-To-Send_** (`RTS`) utilizzando `CSMA/CA`. All'interno di questa richiesta inserirà informazioni relative al tempo stimato per inviare `DATA` e ricevere il relativo `ACK`.

Ricevuto la `RTS`, il nodo `B` invierà, sempre dopo un tempo `SIFS`, un messaggio **_Clear-To-Send_** `CTS` in risposta al `RTS`. Questo messaggio però non sarà inviato solamente ad `A`, ma verrà _**trasmesso in broadcast per tutti i dispositivi che sono in grado di sentirlo**_, cosicché:
- Il nodo `A` potrà procedere ad inviare il _data-frame_ dopo un tempo `SIFS` certo che non avverranno collisioni
- Gli altri nodi staranno in attesa poiché sono stati informati che il nodo `B` è attualmente occupato.

Quando il nodo `B` avrà ricevuto tutti i dati, procederà ad inviare l'`ACK` **nuovamente in _broadcast_**. Alla ricezione di questo `ACK` tutti i nodi vengono a conoscenza che la trasmissione è terminata, e il nodo `B` è nuovamente libero di ricevere altri messaggi. Attenderanno quindi un timer casuale (sempre seguendo `CSMA/CA`) e ricomincieranno a trasmettere.

Questo meccanismo è tuttavia _opzionale_, poiché nel caso in cui il i dati da trasmettere abbiano dimensione comparabile con `RST` e `CTS` l'utilizzo nel `VCS` non è conveniente.

</div>
<div class="">
<figure class="">
<img class="" src="./images/wireless/VCS-example.png">
<figcaption>

Il fatto che `RST` e `CTS` sono piccoli implica non solo che la probabilità di collisione diminuisce, ma che le eventuali ritrasmissioni non influenzino troppo le trasmissioni successive.
</figcaption>
</figure>
</div>
</div>

### 3.3.2. Addressing

I frame `802.11` si basano sui frame `Ethernet`, ai quali aggiungono però campi specifici per l'utilizzo dei link wireless.

<figure class="">
<img class="" src="./images/wireless/WiFi-frame.png">
<figcaption>

I numeri sopra ogni campo indicano la lunghezza il **Byte**.
I numeri sopra i sotto campi del `frame control` rappresentano la lunghezza in **bit**.
</figcaption>
</figure>

Troviamo quindi i seguenti campi:
- **Frame Control** (`2Byte`): si espande in altri campi:
  - _Protocol Version_ (`2bit`): versione del protocollo di comunicazione
  - _Type_ (`2bit`): indica il tipo di frame (`RTS`, `CTS`, `ACK`, `data`)
  - _Subtype_ (`4bit`): permette di specificare ulteriormente il tipo di messaggio
  - _To AP_ (`1bit`): flag che indica se il messaggio è diretto verso un `AP`
  - _From AP_ (`1bit`): flag che indica se il messaggio è proviene da un `AP`
  - _More frag_ (`1bit`): indica se il frame è seguito da altri frammenti o meno
  - _Retry_ (`1bit`): indica se il frame è una ritrasmissione
  - _Power Mgmt_ (`1bit`): indica in che stato deve entrare un `AP` dopo aver ricevuto il frame (_power-save mode_/_active mode_).
  - _More data_ (`1bit`): indica se il trasmettitore ha altri dati da inviare o meno. È particolarmente utile per i _frame_ che raggiungono `AP` in _power-save mode_
  - _WEP_ (`1bit`): indica se il messaggio è cifrato o meno
  - _rsvd_ (`1bit`): bit riservato che indica se i frame ricevuti vanno analizzati strettamente in ordine
- **Duration** (`2Byte`): contiene il periodo di tempo (in millisecondi) per il quale il mezzo si stima verrà occupato
- **Address 1** (`6Byte`): indica l'indirizzo `MAC` del destinatario del messaggio
- **Address 2** (`6Byte`): indica l'indirizzo `MAC` del trasmettitore del messaggio
- **Address 3** (`6Byte`): indica l'indirizzo `MAC` del router al quale l'`AP` è collegato
- **Seq. Control** (`2 Byte`): è l'equivalente del _frame-sequence-number_ per implementare un _reliable-data-trasnfer_
- **Address 4** (`6Byte`): utilizzato solo nella modalità _ad-hoc_.
- **Payload** (`0-2312Byte`): consiste tipicamente di un datagramma `IP` o di un pacchetto `ARP`. Tipicamente la sua dimensione è inferiore ai `1500Byte`.
- **CRC** (`4Byte`): sequenza di `32bit` per consentire il _cyclic-redundancy-check_ per rilevare errori nei bit del messaggio ricevuto. Poiché il `BER` è molto maggiore nelle reti wireless, questo campo assume molta più importanza di quanta non ne avesse nelle reti cablate.


Per comprendere meglio l'utilizzo dei tre indirizzi analizziamo il seguente esempio.

<div class="grid2">
<div class="">

Nella figura sulla destra sono presenti due `AP`, ognuno responsabile di diverse stazioni wireless, ed entrambi connessi direttamente con un _gateway router_ $R_1$.

Quando il router dovrà inoltrare un messaggio verso la stazione wireless $H_1$, lui non sarà a conoscenza del fatto che è presente un `AP` tra lui e l'_host_, ma crede che $H_1$ sia banalmente un _host_ della sottorete alla quale lui fa parte.

Il router, che conosce l'`IP` di $H_1$, utilizza `ARP` per determinarne il `MAC`, incapsulando di conseguenza il datagramma in un `Ethernet Frame` che avrà come campo sorgente il `MAC` di $R_1$ e come campo destinatario l'indirizzo `MAC` di _broadcast_.

Quando il frame ethernet arriva all'`AP`, questo convertirà il frame `802.3 Ethernet` in un frame `802.11` prima di trasmetterlo nel canale wireless, impostando:
- **Address 1**: indirizzo `MAC` _broadcast_
- **Address 2**: il proprio indirizo `MAC` (`AP`)
- **Address 3**: indirizzo `MAC` di $R_1$

Quando tutti i dispositivi avranno ottenuto il messaggio, solamente $H_1$ si riconoscerà nell'indirizzo `IP`, e procederà ad apprendere l'indirizzo `MAC` del router e dell'`AP`.

A questo punto $H_1$ vorrà inoltrare un datagramma verso $R_1$ per rispondere alla richiesta `ARP`. Creerà quindi un frame `802.11` che si trova impostati:
- **Address 1**: indirizzo `MAC` dell'`AP`
- **Address 2**: il proprio indirizo `MAC` ($H_1$)
- **Address 3**: indirizzo `MAC` di $R_1$

Quando l'`AP` lo riceverà procederà con la conversione in _frame_ `802.3` impostando il campo sorgente con il `MAC` di $H_1$ e quello destinatario con quello di $R_1$.

A questo punto il router potrà inoltrare il messaggio a $H_1$ seguendo lo stesso schema di traduzioni nell'`AP`.

</div>
<div class="">
<img class="80" src="./images/wireless/addressing-example.png">
</div>
</div>

### 3.3.3. Mobilità a Subnet Unica

Per aumentare la portata di una rete wireless, un azione comune è quella di introdurre più `BSS` (_Basic-Service-Set_) all'interno della stessa subnet.

Questa scelta comporta però la necessità di dover gestire correttamente la gestione di un dispositivo quando cambia `BSS`, mantenendone la sessione `TCP`.

<div class="grid2">
<div class="">

L'immagine sulla destra mostra due `BSS` interconnessi, ed un _host_ $H_1$ che si sposta da `BSS1` a `BSS2`.

Poichè entrambi i gli `AP` sono connessi allo stesso _gateway router_ tutte le stazioni appartengono alla stesso _subnet IP_.
Ciò implica che l'indirizzo `IP` di $H_1$ rimane invariato, apparendo a tutti gli effetti come fisso per chi è all'esterno della _subnet_.

Quando $H_1$ si sposta lontano da `AP1`, sente che il segnale ricevuto diminuisce, e inizia a cercare _beacon frame_ di altri `AP` più potenti, selezionando `AP2`.

$H_1$ quindi si **dissocia** da `AP1` e si associa con `AP2`.

Il problema di questo cambio si riflette sullo _switch_, che nella propria _tabella di forwarding_ ha scritto che $H_1$ appartiene a `AP1`, e non è a conoscenza del fatto che dovrà adesso instradare a `AP2` i pacchetti diretti a $H_1$.

Una soluzione un po' subdola ma efficace, è impostare gli `AP` affinché ogni volta che un nuovo dispositivo si connette inviino un _Ethernet frame_ in _broadcast_.
In questo modo, quando lo _switch_ riceve questo messaaggio, modifica opportunamente la propria _forwarding table_.

Esistono tuttavia altri protocolli _inter-AP_ cge gestiscono questi problemi in modo più efficace.

</div>
<div class="">
<img class="80" src="./images/wireless/mobility-in-subnet.png">
</div>
</div>

### 3.3.4. Capacità Avanzate

Il protocollo `IEEE 802.11` presenta alcune capacità avanzate che ne permettono una maggiore efficienza.

La prima è la **_rate adaptation_**. I singoli `AP` e nodi mobili cambiano dinamicamente la propria _transmission rate_, sfruttando tecniche di modulazione del livello fisico.

Quando un dispositivo si sposta, più si allontana dalla _base-station_ più il `SNR` diminuisce e il `BER` aumenta.
Quando il `BER` diventa troppo elevato, il dispositivo **diminuisce il proprio _transmission rate_** selezionando un diverso algoritmo `SNR`, diminuendo di conseguenza anche il `BER`.

Una seconda capacità avanzata è il **_power management_**. Nei dispositivi cablati, l'infrastruttura era fissa, e i singoli dispositivi erano connessi alla rete elettrica.
L'impatto energetico del _networking_ era quindi sì considerato, ma non era un problema rilevante. Con il diffondersi dei dispositivi portatili invece il problema di diminuire l'energia consumata diventa molto rilevante, in quanto sono dispositivi a batteria.

Poiché le componenti non sempre sono attive, un operazione comune è quella di diminuire il voltaggio fornito alle interfacce, fino a spegnerle, quando non sono utilizzate.

Ad esempio, i portatili diminuiscono il voltaggio alle interfacce wireless, mettendole in uno stato di _sleep_, quando non devono inviare pacchetti. Quando sarà necessario inviare un pacchetto si riaumenterà il voltaggio rendendo l'interfaccia nuovamente _active_.

Quando invece è necessario ricevere dei pacchetti, il dispositivo, prima di andare in _sleep_, invia un frame all'`AP` indicando la propria scelta di andare in _sleep_ fino al prossimo _beacon frame_.

L'`AP` quindi procederà a **non inoltrare frame al nodo**, ma utilizzerà un sistema simil _proxy_ che tiene traccia dei pacchetti indirizzati al nodo mentre era in _sleep_.

Al successivo _beacon frame_, l'`AP` consulterà una speciale lista di dispositivi in _sleep_ che hanno ricevuto dei frame che non sono ancora stati consegnati.

Alla ricezione di questo _beacon frame_, un singolo _host_ si risveglia e attende di ricevere gli eventuali frame `AP-to-mobile` conservate nel _simil-proxy_. Quando questi terminano, il singolo _nodo_ **tornerà in _sleep_ fino al prossimo beacon**.

Tutto questo introduce un ritardo nella consegna dei pacchetti, anche se tendenzialmente è "limitato".

## 3.4. Reti Wireless `LAN` - `LiFi`

La rete `LiFi` è una tecnologia moderna simile al `WiFi` che non sfrutta però comunicazioni elettromagnetiche ma **_attraverso la modulazione di luce `LED`_**.

<div class="grid2">
<div class="">

Comunicare con la luce, invece dei segnali a radio frequenza, ha diversi vantaggi.

Il primo vantaggio è a livello di **sicurezza**. Infatti la rete `WiFi`, che comunica in _broadcast_, è la più facile da _sniffare_, in quanto la comunicazione in radio frequenza attraversa i muri, comunicando anche al di fuori degli edifici.
La luce infatti rimane all'interno delle stanze chiuse, rendendo impossibile sniffare i pacchetti da fuori.

Un secondo vantaggio è la minore susciettibilità a **interferenze**, in quanto il segnale luminoso è **_immune a interferenze di natura elettromagnetica_**, oltre a **_non generarne_**.
In questo modo si può avere un utilizzo anche in contesti ad alto rischio interferenze, come stanze con grossi aparecchi che operano sfruttando campi magnetici.

Il terzo, e principale vantaggio. è il **minore costo**. Infatti, è possibile integrare questa tecnologia anche nel sistema di illuminazione.

Altri vantaggi sono:
- Permette una comunicazione nell'ordine del gigabit a diversi dispositivi in parallelo
- Non soffre di congestioni
- È una tecnologia molto più affidabile
- Offre una latenza molto più bassa (fino a 3 volte inferiore).


Il setup tipico è quello mostarto nell'immagine sulla destra.

</div>
<div class="">
<img class="40" src="./images/wireless/LiFi-scheme.png">
</div>
</div>

Sono state fatti degli esperimenti, utilizzando un ufficio di $3m\times 3 m$ mettendo il trasmettitore a $2.7m$ sopra un armadio e mettendo diversi ricevitori, uno tenuto in mano a distanza $1m$, uno sulla scrivania a $1.5m$ e uno sul pavimento a circa $2.5m$.

Il primo esperimento è l'impatto sul bit-rate (sia in _downlink_ che in _uplink_) al variare dei dispositivi connessi.

<div class="flexbox" markdown="1">

|    Scenario     | Download (Mbps) | Uplink (Mbps) |
| :-------------: | :-------------: | :-----------: |
| Un dispositivo  |  $\approx 250$  | $\approx 160$ |
| Due dispositivi |  $\approx 125$  | $\approx 80$  |

</div>

Un secondo esperimento è stato in riferimento alla posizione/orientamento del ricevitore rispetto al trasmettitore.

<div class="grid2">
<div class="">
<figure class="90">
<img class="100" src="./images/wireless/LiFi-experiment-1x.png">
<figcaption>

Shift relativo al cono di luce sull'asse $X$
</figcaption>
</figure>
</div>
<div class="">
<figure class="90">
<img class="100" src="./images/wireless/LiFi-experiment-1z.png">
<figcaption>

Shift relativo all'allineamento sull'asse $Z$.
</figcaption>
</figure>
</div>
</div>

L'area di copertura è di circa:
<div class="flexbox" markdown="1">

| Posizione | Distanza $(m)$ | Max $x$ $(m)$ | Max $z$ $(m)$ | Area $(m^2)$ |
| :-------: | :------------: | :-----------: | :-----------: | :----------: |
|  In mano  |     $1.00$     |    $1.10$     |    $0.90$     |    $3.11$    |
|  Desktop  |     $1.50$     |    $1.30$     |    $1.60$     |    $6.26$    |
| Pavimento |     $2.50$     |    $1.70$     |    $1.90$     |   $10.93$    |

</div>


L'ultimo esperimento è stato misurare l'effetto dell'inserimento di ostacoli, divisi in _ostacoli fisici spessi_, _ostacoli fisici "fini"_ e _ostacoli occasionali_.

<img class="60" src="./images/wireless/LiFi-experiment-2.png">


Questo tipo di tecnologia è molto semplice da utilizzare e fornisce alti bit-rate, ma soffre di diversi problemi (_Line-Of-Sight_, _Short Range_, _Shadow Zone_, _Obstacle Sensitivity_).

## 3.5. Reti Wireless `PAN` - `Bluetooth`

Le reti `PAN` (_Personal Area Network_) hanno raggio di copertura minore di $5m$, e sono utilizzate per rimpiazzare tanti collegamenti cablati.

<div class="grid2">
<div class="">

È una tecnologia _ad-hoc_, che non si basa su alcun tipo di infrastruttura, anche se oggi è utilizzato anche per connettersi all'infrastruttura di internet.

Sfrutta la comunicazione attraverso _radio frequenza_, nella banda di `2.4 - 2.5 GHz`.

Inizialmente il _data-rate_ era di `1Mbps` e oggi arriva fino a `3Mpbs`.

L'accesso è multiplo basato su _polling_, dove il _master_ invia un pacchetto di _poll_ ad un dispositivo alla volta, e questo risponde con un pacchetto con dei dati o _null_.

I dispositivi _slave_ si possono dividere in:
- **Client Device**: sono sempre accesi e in attesa del _poll_ del _master_
- **Parked Device**: sono dispositivi che cercano di  risparmiare energia. Ad esempio, possono dormire negli slot nei quali è sicuro che il master non comunicherà.

</div>
<div class="">
<img class="80" src="./images/wireless/PAN-scheme.png">
</div>
</div>

Lo slot dura circa $652 \mu s$, ed è utilizzato in `TDM`, ovvero in un solo senso.
Ad esempio il master trasmette negli slot pari e i client in quelli dispari.

_Bluetooth_, per risparmiare energia, trasmette segnali a potenza estremamente ridotta. Tuttavia, questo comporta che i messaggi possano essere facilmente distrutti dalle interferenze.

Proprio per questo motivo è introdotto il _frequency hopping_. Scegliendo tra 79 possibili canali di frequenza, è possibile per un dispositivo cambiare la propria frequenza in modo pseudo-casuale ad ogni _slot_. Questa sequenza _pseudo-casuale_ viene trasmessa dal _master_ durante il **bootstrap** del dispositivo, seguendo un _seed_ diverso per ogni dispositivo.

In questo modo i tutti dispositivi sono coordinati tra di loro, ed ad ogni _slot_ si trovano sulla frequenza corretta per parlare con il _master_.

Questa scelta implementa anche sicurezza dal punto di vista dello _sniffing_, dato che un malintenzionato sarà costretto ad analizzare tutte le 79 possibili frequenze costantemente e combinare tutti i vari segnali per poter riuscire a recuperare i messaggi trasmessi da i due dispositivi.

## 3.6. Reti Cellulari `WAN` - `4G` e `5G`

Le reti cellulari sono la soluzione per la fornitura di internet in reti `WAN` wireless, e sono oggigiorno più che ampiamente utilizzate.

Infatti la tecnologia `4G` è utilizzata per il $97\%$ del tempo in Corea e più del $90\%$ negli Stati Uniti.

<div class="grid2">
<div class="top">
<p class="p">Similitudini con Internet Cablato</p>

---

Effettua una distinzione tra dispositivi di _edge_ e _core_, ma entrambi appartengono alla stessa famiglia.

Inoltre è anch'essa una _rete di reti_, e sfrutta molti protocolli che abbiamo già visto: `HTTP`, `DNS`, `TCP`, `UDP`, `IP`, `NAT`, `SDN`, `Ethernet`, `tunneling`.

È connesso all'internet cablato.

</div>
<div class="top">
<p class="p">Differenze con Internet Cablato</p>

---

Ha un diverso _wireless link layer_ e ha come obiettivo primario quello della mobilità.

L'identità di un utente è adesso associato ad una `SIM card`, e ha un modello di business diverso, basato sull'iscrizione a dei _cellular provider_.

È basato su strutture di autenticazione e ha una forte notazione di _home network_ rispetto al roaming di reti visitate.
</div>
</div>

---

<div class="grid2">
<div class="">

La _base-station_ viene chiamata `eNode-B` mentre il dispositivo mobile è chiamato `UE` (_User Element_).
Il dispositivo mobile può essere uno smartphone, un tablet, un laptop, una macchina, ...

Questo è adesso identificato non più da un indirizzo `IP`, bensì da un indentificatore a `64bit` chiamato **_International Mobile Subscriber Identity_** (`IMSI`), che è contenuto all'interno della scheda `SIM` (_Subscriber Identity Module_)

La _base-station_ si connette ad una rete `IP` che segue tutti gli standard di internet che abbiamo già discusso.

Troviamo in questa rete:
- **PDN Gateway** `P-GW`: connette la rete ad internet
- **Serving Gateway** `S-GW`: sono dei gateway che permettono l'instradamento dei dati.
- **Home Subscriber Service** `HSS`: matiene i _record_ di tutti gli utenti iscritti
- **Mobility Management Entity** `MME`: mantiene informazioni relative alle celle nei quali si trova un dispositivo in un certo momento. Fonrisce i sistemi di _autenticazione_, di _path setup_ e di _cell location tracking_.

</div>
<div class="">
<img class="80" src="./images/wireless/4G-LTE-architecture.png">
</div>
</div>

Anche `LTE` presenta una separazione tra _control plane_ e _data plane_:
- **Control Plane**: introdotti nuovi protocolli per supportare la mobilità dei nodi, la sicurezza e l'autenticazione.
- **Data Plane**: introdotti nuovi protocolli all'interno del _link layer_ e del _physical layer_. Inoltre si fa un elevato utilizzo del tunneling per semplificare la mobilità.

<div class="grid2">
<div class="">

L'architettura `4G LTE` si basa su quella `IP` che abbiamo già studiato, separando però il _link level_ in tre _sublayer_ (dal basso verso l'alto):
- **Medium Access Control Protocol** (`MAC`): questo layer effettua lo _scheduling_ delle trasmissioni, richiedendo e utilizzando i _radio transmission slot_. Esegue inoltre funzioni di _error detection/correction_, incluso l'utilizzo di _bit di ridondanza_ come tecnica per l'_error correction_ a seconda delle condizioni del canale di comunicazione
- **Radio Link Control Protocol** (`RLC`): questo layer esegue due importanti funzioni:
  - La _frammentazione_ (nel trasmettitore) e il _riassemblaggio_ (nel ricevitore) dei datagrammi `IP` troppo lunghi per essere trasmessi negli _slot_.
  - Implementa il _Reliable link layer data transfer_ utilizzando `ACK/NAK` basati sul protocollo `ARQ`
- **Packet Data Convergence Protocol** (`PDCP`): Effettua la compressione degli _header_ per diminuire il numero di bit trasmessi nel _link_ e la (de)cifratura del datagramma `IP` utilizzando le chiavi ottenute tramite i messaggi scambiati tra il dispositivo e l'`MME` quando il dispositivo si è collegato per la prima volta alla rete.

</div>
<div class="">
<img class="80" src="./images/wireless/4G-LTE-protocol-stack.png">
</div>
</div>

Così come le reti `WiFi` e `Bluetooth`, anche le reti cellulari fanno utilizzo della **_sleep mode_** per preservare la batteria.

In particolare si utilizzano due livelli di _sleep mode_:
- **Light sleep**: I dispositivi entrano in questo stato dopo circa $100ms$ di inattività. Si svegliano periodicamente (tipicamente ogni $100ms$) per controllare eventuali trasmissioni in entrata.
- **Deep sleep**: I dispositivi entrano in questo stato dopo circa $5\text{-}10s$ di inattività. Qualora il dispositivo cambiasse cella mentre è in questo stato, è necessario che ristabilisca l'associazione.

Lo standard `4G LTE` oggi sta venendo rimpiazzato dal `5G` che prometteva di incrementare di 10 volte i picchi di _bitrate_, di diminuire di 10 volte la latenza e di incrementare di 100 volte la capacità di traffico rispetto al `4G`.

Il `5G NR` (_New Radio_) sfrutta due bande di frequenza:
- **FR1**: $450MHz$ - $6GHz$
- **FR2**: $24GHz$ - $52GHz$

Queste bande sono nell'ordine delle onde millimetriche e **non sono retro-compatibili con il `4G`**, in quanto utilizzano altre bande di frequenza.

Il vantaggio di queste frequenze è quello di incrementare di rateo di invio dati, sfruttando anche antenne multi direzionali `MIMO`, ma diminuendo i raggi di copertura.

Infatti le celle `5G` sono molto più piccole, raggiungendo raggi di copertura tra i $10m$ e i $100m$.

Per riuscire ad avere una buona copertura, a differenza del `4G LTE` che richiedeva un numero moderato di _base stations_, il `5G` ne richiede un massivo e denso spiegamento.


# 4. Nodi Mobili

Lo spettro della mobilità, dal punto di vista della rete può essere riassunto come:
- **No mobility**: Un dispositivo si piò muovere tra reti, ma mentre lo fa è spento
- **Little Mobility**: Un dispositivo si può movere all'interno della stessa rete di un provider
- **Medium Mobility**: Un dispositivo si può muovere tra più reti dello stesso provider
- **High Mobility**: Un dispositivo si può muovere tra più reti di più provider diversi mentre _mantiene le connessioni attive_.

Il nostro interesse è concentrato verso una **mobilità medio-alta**.

Un primo approccio che possiamo adottare per per permettere la mobilità è **_delegare il problema ai router della rete_**.
In particolare i _router_ pubblicizzeranno i nomi (che siano indirizzi IP permanenti o numeri identificativi come il numero di telefono) dei dispositivi mobili che li visitano attraverso lo scambio delle _routing table_.
Infatti questa operazione è **supportata nativamente dal routing IP**, e non richiede ulteriori modifiche ai protocolli, se non il fatto che il router deve ora pubblicizzare anche gli indirizzi di _host_ oltre a quelli di _rete_.
Le _tabelle di routing_ indicheranno quindi dove ogni dispositivo è locato attraverso un match dei prefissi.

Questa soluzione, per quanto efficace su piccola scala, **_non è scalabile a miliardi di dispositivi_**.
L'approccio utilizzato quindi **_fa gestire il prolema agli end-system_**, spostando la funzionalità ai "bordi" della rete.
In particolare avremo due approcci di _routing_:
- **Indirect Routing**: la comunicazione tra un corrispondente ad un dispositivo è indirizzata all'_home network_ del dispositivo. Sarà l'_host network_ che la inoltrerà al dispositivo
- **Direct Routing**: un corrispondente prima recupera l'indirizzo del dispositivo e solo successivamente comunica direttamente con esso.

<div class="grid2">
<div class="">

L'_home network_ infatti rappresenta una **_sorgente di informazioni finali riguardanti il dispositivo_**. Contattandolo è possibile ottenere informazioni sulla posizione del dispositivo.
L'_home network_ salva le informazioni relative all'identità di un dispositivo all'interno del proprio `HSS`.

Si dice invece _visited network_ una qualsiasi rete diversa dall'_home network_.
Esiste un accordo tra reti diverse che sancisce che tutte le reti devono fornire acesso a tutti i dispositivi visitatori.

Questo schema si presta bene per le reti cellulari, mentre trova alcune difficoltà ad essere applciato alle reti `WiFi`.

In quest'ultime infatti gli `ISP` non hanno nessun concetto di _home globale_.
Infatti le credenziali di accesso di ogni `ISP` sono diverse, e sono conservate all'interno dei singoli dispositivi.
Reti diverse avranno quindi credenziali diverse.

Esistono però alcune rare eccezioni a questo concetto, come ad esempio la rete `eduroam`, che conserva le credenziali in maniera globale, indipendentemente dalla posizione.

Esiste in realtà un architettura `mobile IP` ma non è utilizzata.

</div>
<div class="">
<img class="80" src="./images/wireless/mobile-network-architecture.png">
</div>
</div>


## 4.1. Routing Indiretto

<div class="grid2">
<div class="">

Quando un dispositivo entra un _visited network_, gli viene assegnato un indirizzo temporaneo dal `NAT` locale.

In particolare, l'`MME` di questa rete si associa con il dispositivo compiendo due azioni:
- Registra la posizione attuale del dispositivo con il nuovo indirizzo.
- Informa l'`HSS` dell'_home network_ del dispositivo, aggiornandolo sulla sua posizione attuale.

Quando un corrispondente vorrà comunicare con il dispositivo utilizzerà come _destination address_ **_l'indirizzo dell'home network del dispositivo_**.

L'_home network gateway router_, quando vedrà in ingresso un pacchetto destinato al dispositivo "in trasferta", invece di inoltrarlo internamente effettuerà un _forwarding_ verso il _gateway router_ del _visited network_, salvato nell'`HSS`.

Sarà quindi quest'ultimo ad inoltrarlo finalmente al dispositivo.

Quando il dispositivo risponderà alla comunicazione lo farà inoltrando il datagramma al _visited network gateway router_, che può quindi scegliere tra due strade:
1. Inoltrarlo **direttamente** al corrispondente: a questo punto il corrispondente potrebbe scegliere persino di passare ad un routing diretto.
2. Inoltrarlo all'_home network gateway router_, che poi lo reindirizzerà al corrispondente.

</div>
<div class="">
<img class="80" src="./images/wireless/mobile-indirect-routing.png">
</div>
</div>

Questo problema di _triangolazione_ può però diventare **inefficiente** qualora il corrispondente e il dispositivo si trovino nella stesse rete.

Infatti la posizione del dispositivo mobile è **_trasparente nei confronti del corrispondente_**, in un approccio _privacy first_.

Abbiamo però il vantaggio che le connessioni _on-going_ possono essere mantenute anche se il dispositivo cambiasse _visited network_, poiché il record dell'`HSS` verrà costantemente aggiornato ad ogni cambio di network.

Durante il cambio si rischia di perdere qualche pacchetto, ma questo è in linea con la filosofia _best-effort_ di internet, e quindi accettabile.

## 4.2. Routing Diretto

<div class="grid2">
<div class="">

Quando un corrispondente vuole comunicare con un dispositivo chiede la posizione attuale al _home network_ del dispositivo, che la fornirà.

Successivamente il corrispondente **comunicherà direttamente** con il dispositivo.

In questo modo, qualora i due dispositivi si trovassero nella stessa rete, si risolve l'inefficienza della triangolazione.

In questo caso però se il dispositivo cambiasse rete le connessioni _on-going_ non sarebbero automaticamente preservate, rendendo necessario definire nuovi protocolli per risolvere questo problema.

Alcune soluzioni poco eleganti possono essere:
- Quando il dispositivo si sposta si notifica il _visited network_ precedente che agisce adesso con un approccio di **routing indiretto**. Ha il problema che se il dispositivo cambiasse nuovamente network, si avrebbe un doppio _routing indiretto_.
- Dopo un certo numero di pacchetti "persi" (poiché il dispositivo di è spostato) il corrispondente chiede nuovamente al `HSS` l'indirizzo del dispositivo. Se fosse lo stesso potrebbe chiudere la connessione, mentre se fosse diverso inizierebbe a comunicare con il nuovo indirizzo.

</div>
<div class="">
<img class="80" src="./images/wireless/mobile-direct-routing.png">
</div>
</div>

## 4.3. Impatto della mobilità sui livelli superiori

A livello logico l'impatto della mobilità è minimo, mentre a livello di prestazioni l'impatto può essere molto rilevante. La probabilità di avere **packet loss/delay** aumenta a causa delle ritrasmissioni. Il `TCP` rischia di interpretare questo aumento di perdite come una congestione, diminuendo la _congestion window_ oltre il necessario.
Questo aumenta il _delay_ anche nel traffico reale, diminuendo il _throughput_ di tutte le connessioni.

Questa reazione, che sarebbe normale e auspicabile nel caso la congestione avenisse davvero, è completamente sproporzionata in relazione a quello che sta realmente accadendo.