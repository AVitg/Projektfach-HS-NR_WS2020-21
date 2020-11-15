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
