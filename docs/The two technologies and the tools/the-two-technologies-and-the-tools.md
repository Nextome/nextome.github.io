# The two technologies and the tools

## Prerequisites
Questo documento può essere letto come primo.
## The two technologies
I due principali sistemi di localizzazione Indoor Nextome si basano su tecnologia Bluetooth e consistono in:

- smartphone tracking --> che permette la localizzazione dello smartphone sul quale è installata un'applicazione
- tag tracking --> che permette la localizzazione di piccoli tag alimentati a batteria

### Smartphone tracking
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>  

Per calcolare la posizione di uno smartphone è necessario installare su di esso una mobile app che integri l'SDK di localizzazione Nextome, grazie al quale lo smartphone è in grado di calcolare la propria posizione elaborando i segnali Bluetooth presenti nell'ambiente. I segnali considerati dall'app di localizzazione saranno solo quelli provenienti dai Beacon installati nell'ambiente e registrati all'interno del Nextome Hub come Beacon ancore di riferimento.
<p style="text-align: center;"><img src="/assets/Technologies and tools/1.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 1: Smartphone tracking architecture.</font> </figcaption></p>

### Tag tracking
<span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

Per calcolare la posizione di un oggetto/ una persona è possibile associare ad esso un tag bluetooth alimentato a batteria. I segnali bluetooth trasmessi dal tag saranno rilevati dalle antenne bluetooth (gateway) installati nell'ambiente e saranno elaborati dal server per ottenere la posizione del tag relativa alla mappa dell'ambiente nel quale si trova. I segnali considerati dal motore di calcolo saranno solo quelli trasmessi/rilevati dai tag/gateway registrati all'interno del Nextome Hub.

<p style="text-align: center;"><img src="/assets/Technologies and tools/2.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 2: Tag tracking architecture.</font> </figcaption></p>

## The tools 
In questa sezione saranno presentati i tool che permettono di interfacciarsi con il Nextome Hub dando la possibilità di utilizzare le tecnologie descritte sopra.

<p style="text-align: center;"><img src="/assets/Technologies and tools/3.png" width="100%"><figcaption style="text-align: center;"> <font size="2"> Figure 3: Overview tools.</font> </figcaption></p>

### Nextome Hub
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

Il Nextome Hub è il cuore del sistema Nextome all'interno del quale risiede il motore di localizzazione, tutte le informazioni necessarie per il calcolo e lo storico dei dati calcolati ove richiesto.

### The interfaces
Per interfacciarsi con il Nextome Hub è possibile utilizzare il Nextome Hub Web e la Venue Configurator sviluppati con interfaccia user friendly dal team Nextome oppure utilizzare direttamente le chiamate APIs che permettono di integrare tutte le funzionalità in altri sistemi.
#### Nextome Hub web
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 



#### Venue Configurator
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 

#### Hub APIs
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 


### The services

#### Sdk and Test App
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span> 

#### Map view
<span style="color:#5BC4F0">:material-cellphone-cog: Smartphone Tracking</span>   <span style="color:#7B29EA">:material-tag: Tag Tracking</span> 





