+++
categories = ["golang", "notes", "link", "Raspberry Pi"]
date = "2016-05-28T09:59:35+02:00"
description = "Link zu der Anleitung von Dave Cheney über die Installation von Golang auf dem Rspberry Pi."
keywords = []
title = "Installation von Golang auf dem Raspberry Pi"

+++

Ich war ziemlich schockiert, als ich festgestellt habe, dass ich über `apt-get` nur Version 1.3 von Golang bekommen habe. Ich bin eigentlich davon ausgegangen, dass ich zumindest die fast schon ein Jahr alte Version 1.5 installiert hatte. 

Jedoch warum sollte es an der Stelle anders sein. Debian benötigt halt eine gewisse Zeit, bis die Packete für aktuellere Versionen freigegeben werden. Warum sollte das bei den Golang Packeten anders sein.

Deshalb eine kurze Notiz an mich selber über eine Build Anleitung für aktuellere Versionen (hier 1.5).

* [Building Go 1.5 on the Raspberry Pi](http://dave.cheney.net/2015/09/04/building-go-1-5-on-the-raspberry-pi)
* [Cross compilation](http://dave.cheney.net/2015/08/22/cross-compilation-with-go-1-5) für eine schnellere Installation.

Eine schnellere Alternative ist, die bereits compilierten Packete von Google zu verwenden. [Sie Antwort von Arjan auf stackexchange.com](http://raspberrypi.stackexchange.com/questions/25956/install-golang-the-easy-way).

```
wget https://storage.googleapis.com/golang/go1.6.2.linux-armv6l.tar.gz 
sudo tar -xzf go1.6.2.linux-armv6l.tar.gz -C /usr/local
sudo chgrp -R staff /usr/local/go
export GOROOT=/usr/local/go
export PATH="$PATH:$GOROOT/bin"
```

