+++
categories = ["golang", "notes", "decorator pattern", "pattern"]
date = "2016-05-19T10:58:32+02:00"
description = " Das flexible Decorator Pattern von @tsenart. Erstmal auf der GopherCon 2015 für http.Client vorgestellt."
keywords = []
title = "Golang Decorator Pattern"

+++

Dieses Pattern wurde auf der GopherCon 2015 in Denver vorgestellt. Hier die Ursprünglichen  [Slides zu Embrace the Interface](https://github.com/gophercon/2015-talks/blob/master/Tom%C3%A1s%20Senart%20-%20Embrace%20the%20Interface/ETI.pdf). Der Vortrag konzentriet sich für meinen Geschmack zuwenig auf die wirklich wesentlichen Punkte.

## Einfacher allgemeiner Algorithmus

Dieses Pattern ermöglicht es eine Methode um beliebige Funktionen zu erweitern. Pro Funktionserweiterung wird eine Funktion verwendet (SingleResponsibility).

1. Erstelle ein Interface, welches die zu erweiternde Methode beinhaltet.
2. Erstelle einen `type` wecher die Methode repräsentiert.
3. Erstelle eine Implementierung der Methode des Interface aus 1. für den Typ aus 2.
4. Erstelle eine `type Decorator` Funktion, welches das Interface aus 1. als Input und als Return beinhaltet.
