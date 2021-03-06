# Szenario - AMB-Ware - 4

Heute machen wir ~~blau~~ bunt!

Aufgabe 1
Installieren Sie einen kompletten Elastic-Stack:
* fügen Sie das Elastic Repository hinzu, cent-os
* nutzen Sie yum zum installieren von Logstash, ElasticSearch und Kibana
* modifizieren Sie die Pipeline-Konfiguration unter /etc/logstash/pipelines.yml, so dass diese genau "auf eine Datei zeigt"
* konfigurieren Sie, falls nötig Elasticsearch, Kibana und Logstash (Aufgabe2) (siehe auch hints)
* Richten Sie diese 3 als Dienste ein und starten Sie diese
* tunneln Sie Port 5601 via ssh zu Ihrem Server, um Kibana zu erreichen

Aufgabe 2
"konfigurieren" Sie logstash derart, dass 
* es auf udp oder tcp Port 22514 lauscht
* es die Sample logs [iptables](/Termine/2020_11_06/log_samples) parst (mindesten IP und PORT, die andren Felder sind Kür)
* Wählen Sie die Feld Namen nach [Elastic Common Schema 1.7](https://www.elastic.co/guide/en/ecs/current/index.html)

## hints
* kibana.yml
  * server.host: "0.0.0.0"
* iptables -L -n
  * permanent hinzufügen bitte, ggf ( firewall-cmd --zone=public --add-port=22514/tcp --permanent ) 
  * systemctl restart firewalld
* logstash
  * Pipeline Konfiguration unter /etc/logstash/conf.d/(z.b. iptables.conf)
  * output elasticsearch
	* index => "iptables-%{+YYYY.MM.dd}" (Details später) 
  * Notation nicht source.address, sondern [source][address] 
    * https://github.com/elastic/logstash/issues/8772
  * tail -F /var/log/logstash/logstash-plain.log
  * kein stdout mehr... also zum Testen evtl. die Logstash Version nutzen die wir heruntergeladen haben (Achtung Port nicht doppelt belegen) 
  * Zusatz-"Aufgabe" grokparsefailure handling
    * https://discuss.elastic.co/t/-grokparsefailure-general-exception-handling/80143
  
  
 Teaser...
* [nicht_ECS_konform](https://github.com/AVitg/Projektfach-HS-NR_WS2020-21/edit/master/Termine/2020_11_20/ziel.png)
