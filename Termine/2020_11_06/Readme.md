# Szenario - AMB-Ware - 2
In Abschnitt 1 haben wir festgestellt, 
* dass nicht nur PCs, sondern auch Drucker, Türschlösser Gegensprechanlagen, Telefone, Überwachungskameras, Zugangs-Kontrollsysteme und viele Dinge IP Adressen haben.
Netzwerkgeräte wie Firewall, Router, IDS/IPS, WAF, Authentication Authorization Accounting sog. AAA -Syteme ( z.B Windows Active Directory) existieren
* WebServer,  FileServer, Code-Versionierung Server eingesetzt werden können
* verschidene Client und Server BetriebSysteme zum Einsatz kommen können, wie MS Windows, Linux derivate (z.B. RedHat), Mac OS usw.

**All Diese Geräte können Ereignis-Logs erzeugen.**

Beispielhaft wollen wir uns einige Logs kurz anschauen:
Github --> Link zu GitHub 

Wir haben gesehen, dass all diese Logs Gemeinsamkeiten, aber auch Unterschiede haben. 
Sowohl innerhalb eines Log Typs, als natürlich auch zwischen den unterschiedlichen "Herstellern" und "Device" Typen.

Sie fragen ihren Freund, ob er irgendeine Idee hat, wie man diese Logs zentral speichern kann, um diese später analysieren zu können.
Daraufhin sagt ihr Freund, dass nicht nur ein Zentrales Speichern wichtig ist, sondern auch die Möglichkeit diese Logs später zu Durchsuchen zu filtern und zu sortieren.

Schauen wir uns "der Einfachheit halber" nochmal die iptables logs genau an und separieren wir die Felder.
"Excel" ist hier unser Freund? 
## Aufgabe 
Mit Hilfe eines Tabellenkalkulationsprogramm einzelne Felder separieren, sortieren, filtern... und natürlich Überschriften für die Spalten
Sample Hier

Gruppenarbeit bitte ;) 

 XLS File von iptables 

# Szenario - AMB-Ware - 2.1
Eine automatische "Normalisierung" ist natürlich mit Excel und Co nicht möglich.
Wonach haben Sie in der vorherigen Übungssequenz die die Felder Unterteilt?
Kann man das allgemeingültig (für iptables-logs) definieren?
Wie?

## Am besten mit regulären Ausdrücken/ regex 

Links zu regex:

* https://github.com/aloisdg/awesome-regex
* https://github.com/kkos/oniguruma/blob/master/doc/RE
* Unter Summary nochmal ein paar Beispiele https://coralogix.com/log-analytics-blog/regex-101/

versuchen Sie "einen" regulären Ausdruck zu finden, um die iptables logs in einzelne Match Groups zu zerlegen. 

## Logstash 
Logstash ist Teil des ELK-Stacks und kann u.a. 
* Daten via TCP/UDP empfangen, 
* Daten Manipulieren/Parsen
* Daten auf verschiedene Wege "ausgeben"


https://artifacts.elastic.co/downloads/logstash/logstash-7.9.3.tar.gz

``` 
./logstash-7.9.3/bin/logstash -f  logstash-7.9.3/config/<your.config.here>.conf --config.reload.automatic
```

https://www.elastic.co/guide/en/logstash/7.9/plugins-filters-grok.html#_custom_patterns
https://github.com/logstash-plugins/logstash-patterns-core/blob/master/patterns/grok-patterns
