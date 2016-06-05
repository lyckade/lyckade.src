+++
categories = ["golang", "notes"]
date = "2016-06-05T19:05:37+02:00"
description = "Vererbung gibt es in Go eigentlich nicht. Das Konstrukt der Wahl ist die Komposition. Hier ein kleines Beispiel."
keywords = []
title = "Vererbung in Go"

+++

Es gibt eine Klasse, welche als Basis für eine Neue Klasse dienen soll. Für dieses Beispiel `Car`. Über eine Funktion wird ein Pointer auf eine Instanz erzeugt:

```go
type Car struct {
	speed int
	name  string
}

func (c *Car) Drive (){
	fmt.Println("Brum brum")
}

func NewCar() *Car {
	car := Car{speed: 100, name: "fastcar"}
	return &car
}
```

Um alle Eigenschaften der ursprünglichen Klasse der neuen Klasse hinzuzufügen fügen wir `Car` der neuen Klasse einfach über das struct hinzu. Die Klasse `Audi` erbt nun alle Eigenschaften und Methoden von `Car`. Wenn wir hierfür auch eine Funktion verwenden wollen, welche eine Referenz auf eine Instanz `Audi` erzeugt, dann lässt sich dies auch recht einfach umsetzen. Hierfür setzt man `audi.Car = *car` 

```go 
type Audi struct {
	Car
}

func NewAudi() *Audi {
	car := NewCar()
	var audi Audi
	audi.Car = *car
	audi.name = "Audi"
	return &audi
}
```
https://play.golang.org/p/jo9dByX8sX

Eine praktische Anwendung ist hierbei die Erweiterung der Standardbiliothek. 