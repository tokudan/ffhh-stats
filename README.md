Freifunk stats
--------------

Inhalt:
scripts/
	query-data.influx
		Skript das die Daten sammelt und an InfluxDB schickt
	example.conf
		Beispiel-Konfigurationsdatei für das query-data Skript
	influxdb-cq.txt
		Beispiel-Queries die in InfluxDB genutzt werden können um Statistiken nach Regionen zusammenzufassen
		und ein Downsampling aufzubauen.

Anzupassende Konfiguration:
* scripts/example.conf


Abhängigkeiten
--------------
- Ruby 1.9+ (getestet nur mit ruby 2.1.5)
	https://www.ruby-lang.org/
- Meshviewer (genauer: kompatible Quell-Dateien nodes.json und graph.json)
	https://github.com/tcatm/meshviewer
- InfluxDB
	https://github.com/influxdata/influxdb
- wget

Empfehlung:
- Grafana zur Anzeige der Daten


Befehlszeilenoptionen
--------------
Das Skript unterstützt drei Argumente:
--config <example.conf>
	Legt den Pfad zur Konfigurationsdatei fest, muss angegeben werden
--dry-run
	Sendet keine Daten an Carbon (die Verbindung zu Carbon wird aufgebaut, es werden aber keine Daten gesendet)
--debug
	Zeigt die Daten an, die an Carbon gesendet werden (bzw. gesendet würden, falls gleichzeitig --dry-run genutzt
	wird). Skript wird nach einem Durchlauf beendet, statt in eine Endlosschleife zu gehen.


Installation & Konfiguration
----------------------------
Hinweis: Das Skript benötigt keine temporären Verzeichnisse o.ä. und schreibt nirgends ins Dateisystem, kann also
entsprechend restriktiv behandelt werden. Zum Download der Quelldaten wird wget genutzt.

1. Konfigurationsdatei anpassen
graph_json: URL zur graph.json (wird an wget übergeben)
nodes_json: URL zur nodes.json (wird an wget übergeben)
alfred_json: URL zur alfred.json (optional, für traffic-statistiken, wird an wget übergeben)
interval: Interval in der die JSON-Dateien erneut heruntergeladen und ausgewertet werden sollen
carbon_host: IP-Addresse des Graphite-Ports von InfluxDB
carbon_port: Port des Graphite-Ports von InfluxDB
region: Region die an InfluxDB gemeldet werden soll (z.B. ffhh für Freifunk Hamburg, evtl. Escape-Regeln von InfluxDB beachten, alphanumerisch geht immer)
groups_dir: Verzeichnis in dem manuell definierte Knotengruppen gespeichert sind (kann ein leeres Verzeichnis sein,
Beispiel: https://github.com/tokudan/ffhh-statsgroups)

2. Skript und Konfiguration testen
$ ./query-data.influx --config /pfad/zur/example.conf --dry-run --debug
Hierdurch wird das Skript mit der angegebenen Konfigurationsdatei gestartet, baut auch eine Verbindung zu InfluxDB auf,
 sendet aber keine Daten. Gleichzeitig werden die Daten die gesendet würden angezeigt.
Wenn alles wie gewünscht ausgegeben wird, kann das Skript ohne --dry-run und --debug gestartet werden und damit die
Integration in InfluxDB getestet werden.

3. Grafana konfigurieren
[TODO]


Lizenz
------
GPL2 oder neuer
