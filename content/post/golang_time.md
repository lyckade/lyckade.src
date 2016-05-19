+++

date = "2016-05-15T15:37:02+02:00"
description = "Eine kurze Einführung in das time package"
title = "Golang und die Zeit: Das magische Datum"
categories =["golang","notes","time"]

+++

Die Dokumentation zu dem Packet time ist bezüglich der Formatierung des Datums nicht sehr ausführlich. Deshalb soll die fast schon geniale Logik kurz vorgestellt werden.

## Das magische Datum

Der Ansatz von go ist anders als bekannt. Die Definition des Datum-Format erfolgt dabei nicht über Flags sondern über ein fest eindeutig definiertes Datum.

Die Dokumentation hat in dem Packet bereits ein paar Konstanten definiert: https://godoc.org/time#pkg-constants

> ANSIC       = "Mon Jan _2 15:04:05 2006"

An den Konstanten erkennt man das magische Datum: der 2.01.2006 um 15:04:05. In einer anderen Darstellung kann man sehen, dass dies die Zahlen von 1 bis 7 sind:

> 01/02 03:04:05PM '06 -0700

Dadurch wird das jeweilige Element für die Formatierung identifieziert.

Möchte ich nun einen einfachen Zeitstempel erzeugen kann ich das Layout ganz einfach mit dem Datum definieren:

``` go
const timestampLayout = "20060102150405"
```

Die Ausgabe erfolgt nun ganz einfach über `Format()`. Das Beste an der Stelle ist, dass dies mit der Funktion `time.Parse()`  auch in die andere Richtung funktioniert.

Auf dem Playground gibt es das ganze noch einmal als komplettes Beispiel:

https://play.golang.org/p/n0YuYWwolV



