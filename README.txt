R4HTTP.R4P
===========

R4HTTP ist der wiederverwendbare HTTP/1.1-Protokollbaustein von R4OS.
Er verwendet den gemeinsamen SDK-Kern `r4os.http`; Netzwerkzugriffe selbst
bleiben Aufgabe der SDK-Webtransport-Fassade ueber R4NET.

Rolle: application.http

Operationen:
- 1: Faehigkeiten
- 2: GET-Request aus einer absoluten HTTP-/HTTPS-URL bauen
- 3: vollstaendige HTTP-Antwort dekodieren
- 4: Redirectziel relativ zu einer Basis-URL aufloesen
- 5: deterministischer Selbsttest

Der Parser begrenzt Headerzahl und Headergroesse, unterstuetzt
Content-Length, Chunked Transfer und close-delimited Antworten und lehnt
mehrdeutige Content-Length-/Transfer-Encoding-Kombinationen ab.

Seit 0.63.15 stellt der gemeinsame Kern zusaetzlich einen inkrementellen
Content-Length-Decoder fuer `App.web().download` bereit. Er behaelt nur den
begrenzten Header im caller-owned Puffer und reicht Bodysegmente unmittelbar
an den durablen Sink weiter. `Content-Range`, 206 und 416 werden strikt
gelesen; ein Resume muss exakt am angeforderten Offset und an derselben
Gesamtgroesse beginnen. Der normale Komplettdecoder bleibt fuer kleine
Antworten und Chunked Transfer bestehen.
