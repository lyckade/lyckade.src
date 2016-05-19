+++

date = "2016-05-17T10:07:30+02:00"
description = "Wie clients mit httptest getestet werden können."
keywords = ["httptest"]
categories =["golang","tests","http","notes"]
title = "Client tests in Golang mit httptest"

+++

Bei der Erstellung von Microservices mittels eine REST API gibt es bei der Kommunikation zwischen zwei Diensten neben dem Empfänger (Server) immer auch eine sendende Seite (Client).

Für das Testen von http Anfragen an einen Server bietet das [httptest Packet](https://golang.org/pkg/net/http/httptest/#Server) einen eigenen Testserver an. Mit der Funktion `NewServer()` lässt sich der Testserver starten. Dabei wird eine Referenz auf eine Server instanz zurück gegeben.

Der Client muss nun für den Test die richtige URL verwenden. Die Testserver URL kann über den Parameter `URL` abgefragt werden.

``` Go
// Neuen Testserver erstellen
ts := httptest.NewServer(http.HandlerFunc(
	func(w http.ResponseWriter, r *http.Request) {
		// Unterschiedliche Response Szenarien können 
		// über den Header einfach erzeugt werden. 
		w.WriteHeader(200)
		fmt.Fprintln(w, "Hallo Client")
}))
defer ts.Close()
// URL des Servers ausgeben
fmt.Println(ts.URL)
```
https://play.golang.org/p/VmZriIZhLm

Damit der Code auch testbar ist, muss beim Client die Möglichkeit bestehen die URL für die Abfrage zu manipulieren. Dafür verwendet man für das Erzeugen der URL am Besten das zugehörige [Packet URL](https://golang.org/pkg/net/url/#URL). Der Datentyp URL zerlegt eine URL in folgende Teile: 

``` 
scheme://[userinfo@]host/path[?query][#fragment]
```


Durch das Setzen des Parameters `host` kann somit nur der relevante Teil ausgetauscht werden. Bei einem GET Aufruf kann das ganze wie folgt aussehen:

``` go
func getInfo(url url.URL) *http.Response{
	myClient := new(http.Client)
	resp, err := myClient.Get(url.String())
	if err != nil {
		// Fehlerbehandlung
	}
	return resp

var reqURL = url.Parse("http://myserver:1234/")
func main(){
	resp := getInfo(reqURL)
	// Client code ...
}
``` 



