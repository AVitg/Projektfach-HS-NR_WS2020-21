# Szenario - AMB-Ware - 3

Beim letzten Termin haben wir Regex und Logstash "kennengelernt".
Hier finden Sie Beispiel Konfigurationen für eine sogenannte Pipeline.
* Beispiel 1 enthält dabei unsere selbst erstellen Regulären Ausdrücke;
* Beispiel 2 verfolgt den Ansatz mit vor-definierten Grok-Patterns zu arbeiten. 

* https://www.elastic.co/guide/en/logstash/7.9/plugins-filters-grok.html#_custom_patterns
* https://github.com/logstash-plugins/logstash-patterns-core/blob/master/patterns/grok-patterns

* logstash_beispiel_1.conf
* logstash_beispiel_2.conf

Verwendien Sie die **zweite** Beispieldatei und starten Sie logstash:
```
./logstash-7.9.3/bin/logstash -f  logstash-7.9.3/config/<your.config.here>.conf --config.reload.automaticMarkdownLivePreview
```

## Aufgabe:

Bearbeiten Sie in einem zweiten Fenster (z.B. [tmux](link) str+b c, oder strg+b ", oder strg+b %) die logstash Pipeline derart, dass die Felder Source & Destination Port, LEN und Protocol aus dem Beispiel in eigene Felder geparst werden.

```
echo "Jan  8 03:37:09 example-host kernel: iptables DROP_INPUT: IN=eth0 OUT= MAC=90:10:35:5a:1e:3a:90:10:9e:ec:2c:71:08:00 SRC=203.0.113.36 DST=172.16.54.114 LEN=52 TOS=0x00 PREC=0x00 TTL=115 ID=15743 DF PROTO=TCP SPT=17805 DPT=445 WINDOW=8192 RES=0x00 SYN URGP=0" > /dev/udp/localhost/22514
```

oder:

```
echo "Jan  8 03:37:09 example-host kernel: iptables DROP_INPUT: IN=eth0 OUT= MAC=90:10:35:5a:1e:3a:90:10:9e:ec:2c:71:08:00 SRC=203.0.113.36 DST=172.16.54.114 LEN=52 TOS=0x00 PREC=0x00 TTL=115 ID=15743 DF PROTO=TCP SPT=17805 DPT=445 WINDOW=8192 RES=0x00 SYN URGP=0" > /dev/tcp/localhost/22514
```

je nach "Input" in Ihrer Pipeline.


**Wie** haben Sie die Felder benannt? In der Übung "parsen via Tabellenkalkulation" haben schon einige festgestellt, dass keine Namen für die Felder gefunden werden konnten/ Hintergrundwissen über den genauen Inhalt der Logs fehlte.

Erlangen sie mit Hilfe ihrer bevorzugten Suchmaschine Hintergrundwissen über die nachstehende Logzeile:
was bedeuten die IP Adressen in Klammern?

```
Oct 10 2018 12:34:56 localhost CiscoASA[999]: %ASA-6-302013: Built outbound TCP connection 11777 for outside:100.66.133.112/80 (100.66.133.112/80) to inside:172.31.98.44/1453 (172.31.98.44/1453)
```

Wie würden Sie diese Felder benennen?

 Jeder von Ihnen hat vermutlich einen eigenen Namen für die Felder gewählt... 
... wäre es nicht toll, wenn es einheitliche Log Formate gäbe?

Das haben sich schon viele gedacht und immer neue Standards erfunden
Exkurs Log Formate

## Aufgabe:
Parsen sie mit Hilfe der Offiziellen Referenz der ECS Version 1.6 die Felder Source, Destination, Protocol, Event Name/Device Action der Beispiel-Logs
Zeitfenster 45 Minuten?
