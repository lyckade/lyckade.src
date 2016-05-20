+++
categories = ["golang", "notes", "decorator pattern", "pattern"]
date = "2016-05-19T10:58:32+02:00"
description = " Das flexible Decorator Pattern von @tsenart. Erstmal auf der GopherCon 2015 für http.Client vorgestellt."
keywords = []
title = "Golang Decorator Pattern"

+++

Dieses Pattern wurde auf der GopherCon 2015 in Denver vorgestellt. Hier die Ursprünglichen  [Slides zu Embrace the Interface](https://github.com/gophercon/2015-talks/blob/master/Tom%C3%A1s%20Senart%20-%20Embrace%20the%20Interface/ETI.pdf). Der Vortrag konzentriet sich für meinen Geschmack zuwenig auf die wirklich wesentlichen Punkte.

### Einfacher allgemeiner Algorithmus

Dieses Pattern ermöglicht es eine Methode um beliebige Funktionen zu erweitern. Pro Funktionserweiterung wird eine Funktion verwendet (SingleResponsibility).

1. Erstelle ein Interface, welches die zu erweiternde Methode beinhaltet.
2. Erstelle einen `type` welcher die Methode repräsentiert.
3. Erstelle eine Implementierung der Methode des Interface aus 1. für den Typ aus 2.
4. Erstelle eine `type Decorator` Funktion, welches das Interface aus 1. als Input und als Return beinhaltet.

### Decorator Gerüst für http.Client

In dem Vortrag wird ein Decorator für `http.Client.Do(*http.Request)` als Beispiel erstellt. Hier wollen wir nun einen eigenen Typ verwenden:

```Go
// Typ me ist Grundlage für das Beispiel.
type me struct{}

// Die Methode Do soll dabei durch einen Decorator erweitert werden.
func (m *me) Do(s string) string {
	fmt.Println("Ich mache etwas:")
	fmt.Println(s)
	out := "Fertig!\n------------------"
	return out
}
```
Das Grundgerüst für den Decorator sieht in dem Fall wie folgt aus:

```Go
// Schritt 1: Erstelle ein Interface, welches die zu erweiternde Methode
// beinhaltet.
type myInterface interface {
	Do(string) string
}

// Schritt 2: Erstelle einen `type` welcher die Methode repräsentiert.
type MyFunc func(string) string

// Schritt 3: Erstelle eine Implementierung der Methode des Interface
// aus 1. für den Typ aus 2.
// Bei der Implementierung wird die Methode Do auf die eigentliche Funktion
// gemapt. D.h. die Methode Do entspricht eigentlich der Funktion.
func (f MyFunc) Do(s string) string {
	return f(s)
}

// Schritt 4: Erstelle eine `type Decorator` Funktion, welches das
// Interface aus 1. als Input und als Return beinhaltet.
type Decorator func(myInterface) myInterface

```

### Erstellen der Decorator Funktionen

Dier Erstellung eines Decorators als Funktion beinhaltet nun

```Go
// Erstellen eines Dekorators:
// Definiere eine Funktion, welche einen Decorator zurück gibt
func myDecorator(d string) Decorator {
	// Der Decorator nimmt myInterface als Input und gibt
	// dieses dekoriert wieder zurück.
	return func(i myInterface) myInterface {
		return MyFunc(func(s string) string {
			fmt.Println(d)
			s = "Noch was"
			return i.Do(s)
		})
	}
}
```

Das Beispiel Komplett im Playground: https://play.golang.org/p/fD8bqoFqeQ

### Anwendung

Durch diese Aufteilung lassen sich unterschiedliche Aufgaben in eigene Funktionen aufteilen. Diese können separat sehr einfach getestet werden, das für den Test als Basis nur das definierte Interface verwendet werden muss.
