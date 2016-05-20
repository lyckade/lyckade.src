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

In dem Vortrag wird ein Decorator für `http.Client.Do(*http.Request)` als Beispiel erstellt. Die vier Schritte sehen als Code dann wie folgt aus:

```Go
// A Client sends http Request and returns response and error
type Client interface {
	Do(*http.Request) (*http.Response, error)
}

// ClientFunc is a custom type for the implementation of the
// Client interface
type ClientFunc func(*http.Request) (*http.Response, error)

// Do implements the Client interface for the ClientFunc
func (f ClientFunc) Do(r *http.Request) (*http.Response, error) {
	return f(r)
}

// Decorator wraps a Client with extra behaviour
type Decorator func(Client) Client

```

### Erstellen der Decorator Funktionen

Dier Erstellung eines Decorators als Funktion beinhaltet nun

```Go
func MyDecorator(l *log.Logger) Decorator {
  // Als Rückgabe wird eine Funktion vom Typ Decorator verwendet
	return func(c Client) Client {
    // Die Decorator Funktion gibt selber wieder eine Funktion vom Typ
    // ClientFunc zurück.
		return ClientFunc(func(r *http.Request) (*http.Response, error) {
			// Ausführen einer zusätzlichen Funktion
      l.Println("Ausgabe aus dem Decorator")
      // Aufruf der zu dekorierenden Funktion über das Interface
			return c.Do(r)
		})
	}
}
```

### Test der Decorator Funktionen


1) Ausgangslage: https://play.golang.org/p/gwiajfcoNa
2) Fertig: https://play.golang.org/p/SanDbt_HhT
https://play.golang.org/p/Drw62pH2zg
https://play.golang.org/p/Jeof7B9L_Y
