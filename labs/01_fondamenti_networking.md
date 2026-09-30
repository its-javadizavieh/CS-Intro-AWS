# Lab 01 - Fondamenti di Networking

## Obiettivo

Rilevare configurazione, gateway, DNS e percorso dei pacchetti del proprio host usando comandi ripetibili, senza modificare la rete.

## Durata (timebox)

90 minuti: 15 preparazione, 45 raccolta dati, 20 diagnosi, 10 cleanup e consegna.

## Prerequisiti

Un computer Ubuntu, macOS o Windows PowerShell con accesso di rete. Non servono privilegi amministrativi. Creare una cartella di lavoro chiamata `lab01-networking` nella propria cartella Documenti.

## Scenario

Un client raggiunge un servizio Internet. Occorre documentare indirizzo locale, gateway, DNS e hop osservati, distinguendo rete locale e rete esterna senza cambiare configurazioni.

## Step (numerati)

1. Creare la cartella `Documenti/lab01-networking` e aprire lì il terminale.
2. Creare `consegna01.md` e inserire: sistema operativo, data/ora, tipo di collegamento (`Ethernet` o `Wi-Fi`).
3. Ubuntu: aprire `Impostazioni -> Rete -> Ingranaggio` per Ethernet oppure `Impostazioni -> Wi-Fi -> Ingranaggio della rete connessa`; annotare IPv4, gateway e DNS.
4. macOS: aprire `Impostazioni di Sistema -> Rete -> Wi-Fi/Ethernet -> Dettagli -> TCP/IP` e poi `DNS`; annotare IPv4, router e server DNS.
5. Windows: aprire `Impostazioni -> Rete e Internet -> Wi-Fi/Ethernet -> Proprietà hardware`; annotare indirizzo IPv4, gateway e DNS.
6. Salvare l’output della configurazione in `configurazione.txt` con uno dei comandi seguenti.
7. Ubuntu: `ip address > configurazione.txt` e `ip route >> configurazione.txt`.
8. macOS: `ifconfig > configurazione.txt` e `route -n get default >> configurazione.txt`.
9. Windows PowerShell: `Get-NetIPConfiguration | Format-List * > configurazione.txt`.
10. Identificare l’interfaccia attiva: tipicamente `eth0`/`enp...`/`wlan0` su Ubuntu, `en0` su macOS, `Ethernet` o `Wi-Fi` su Windows.
11. Sostituire `GATEWAY` con l'indirizzo rilevato al punto 7, 8 o 9, senza parentesi. Salvare quattro richieste: Ubuntu/macOS `ping -c 4 GATEWAY > ping-gateway.txt 2>&1`; Windows `ping -n 4 GATEWAY > ping-gateway.txt 2>&1`.
12. Salvare la prova Internet: Ubuntu/macOS `ping -c 4 1.1.1.1 > ping-internet.txt 2>&1`; Windows `ping -n 4 1.1.1.1 > ping-internet.txt 2>&1`.
13. Risoluzione DNS: `nslookup example.com > dns.txt 2>&1` su tutti i sistemi. Separare risposta del resolver e risposta ICMP.
14. Percorso: Ubuntu/macOS `traceroute example.com > percorso.txt 2>&1`; Windows `tracert example.com > percorso.txt 2>&1`.
15. Se `traceroute` non è installato su Ubuntu, non installare pacchetti senza autorizzazione: usare `tracepath example.com`; se nessuno dei due è disponibile, documentare il limite.
16. In `consegna01.md`, creare una tabella con `Elemento`, `Valore`, `Evidenza`: interfaccia, IPv4, prefisso, gateway, DNS, latenza media al gateway, latenza media a Internet, primo hop e ultimo hop visibile.
17. Aggiungere una diagnosi simulata: gateway raggiungibile ma ICMP esterno senza risposta lascia aperte ipotesi di filtro o percorso; IP esterno raggiungibile ma `nslookup` fallito suggerisce di esaminare il resolver. Un ping fallito verso un nome, da solo, non prova un guasto DNS.

## Svolgimento guidato

Percorso atteso: `client -> interfaccia locale -> gateway predefinito -> rete del provider -> destinazione`.

Parametri da non modificare:

- indirizzo IP e prefisso assegnati;
- gateway predefinito;
- server DNS;
- configurazione Wi-Fi/Ethernet.

Comandi di sola lettura consigliati:

```text
ip address
ip route
ping -c 4 GATEWAY
ping -c 4 1.1.1.1
nslookup example.com
traceroute example.com
```

Su Windows sostituire `ping -c 4` con `ping -n 4` e `traceroute` con `tracert`.

## Output atteso

Cartella `Documenti/lab01-networking` contenente `consegna01.md`, `configurazione.txt`, `ping-gateway.txt`, `ping-internet.txt`, `dns.txt` e `percorso.txt`.

## Checkpoint

Il gateway appartiene alla rete locale; la risoluzione di `example.com` restituisce almeno un indirizzo; il percorso contiene il gateway come primo hop visibile oppure documenta che gli hop ICMP sono filtrati.

## Troubleshooting rapido

- `Request timed out` sul traceroute non prova da solo un guasto: alcuni router filtrano ICMP.
- Gateway assente: verificare di avere selezionato l’interfaccia attiva.
- `nslookup` fallisce ma `1.1.1.1` risponde: controllare il DNS, senza cambiarlo.
- VPN aziendale attiva: annotarla perché può modificare gateway e percorso; non disconnetterla se richiesta dall’organizzazione.

## Cleanup obbligatorio

Non sono state create risorse cloud né cambiate impostazioni di rete. Chiudere i terminali, rimuovere eventuali file di prova diversi dai sei richiesti e conservare soltanto la cartella di consegna. Non pubblicare indirizzi IP privati o nomi host personali.

## Parole chiave Google (screenshot/guide)

- Windows Get-NetIPConfiguration gateway DNS
- Ubuntu ip route default gateway
- macOS network TCP IP DNS details
- traceroute hop timeout ICMP
