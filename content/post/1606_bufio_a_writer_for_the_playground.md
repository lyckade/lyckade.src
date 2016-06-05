+++
categories = ["golang", "notes" ]
date = "2016-06-05T18:31:29+02:00"
description = "Für einfache Tests benötigt man öfters einen Writer, welcher den Output an die Konsole schickt. Hierfür eignet sich sehr gut das bufio Packet in Verbindung mit os."
keywords = []
title = "bufio ein Reader und Writer für den Playground"

+++

Für einfache Tests benötigt man öfters einen Writer, welcher den Output an die Konsole schickt. Hierfür eignet sich sehr gut das bufio Packet in Verbindung mit os.

Das notwendige Beispiel befindet sich sogar schon direkt in der Dokumentation: https://godoc.org/bufio#example-Writer

```go
w := bufio.NewWriter(os.Stdout)
fmt.Fprint(w, "Hello, ")
fmt.Fprint(w, "world!")
w.Flush() // Don't forget to flush!
```

https://play.golang.org/p/VZ7bbaI53z

Indem der Inhalt des Writers einfach an den os.Stdout geleitet wird, können so die Inhalte eines beliebigen os.Writers als Output im Playground sichtbar gemacht werden.