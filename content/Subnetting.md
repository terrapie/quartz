
Eine Firma bekommt das Netz **192.168.0.0/23**.

Es sollen **6 Standorte** eigene Teilnetze bekommen. Jeder Standort braucht **mindestens 25 nutzbare Hosts**.

a) Welche Subnetzmaske ist geeignet?  
b) Wie viele Teilnetze entstehen insgesamt mit dieser Maske?  
c) Geben Sie Netzadresse, erste/letzte nutzbare Host-Adresse und Broadcast-Adresse für das **4. Teilnetz** an.



![[Pasted image 20260910203233.png]]

a)
**Netzwerk komplett:** bis 255 → Broadcast → **/24**

**1. Aufteilung:** bis 127 Broadcast → **/25**

**2. Aufteilung:** bis 63 Broadcast → **/26**

**3. Aufteilung:** bis 31 Broadcast → **/27**


**Also ist die Subnetzmaske: /27**


b)
11111111.11111111.11111110.00000000

2^9 = 512 Hostanteil

512 / 32 = **16 Teilnetze**


c) **4. Teilnetz**

Netzadresse: 192.168.0.96

Hostadresse: 192.168.0.97 - .126

Broadcast-Adresse: 192.168.0.127

---
# VLSM

Ein Unternehmen verwendet das Netzwerk:

**192.168.50.0/24**

Es gibt drei Abteilungen:

- **Verwaltung:** 50 Geräte
- **Entwicklung:** 25 Geräte
- **Support:** 10 Geräte

Das Netzwerk soll so aufgeteilt werden, dass **jede Abteilung ihr eigenes Subnetz** bekommt.

**1.** Welche Subnetzmaske bzw. Präfixlänge benötigt die **Verwaltung** mindestens?

**2.** Welche Präfixlänge benötigt die **Entwicklung** mindestens?

**3.** Welche Präfixlänge benötigt der **Support** mindestens?

**4.** Erstelle eine mögliche Aufteilung des Netzwerks. Gib für jedes Subnetz an:

- Netzwerkadresse
- Präfix
- 1. Host
- letzter Host
- Broadcast

1. Verwaltung benötigt Subnetzmaske /26
2. Entwicklung benötigt Subnetzmaske /27
3. Support benötigt Subnetzmaske /28

**Verwaltung**

**192.168.50.0/24**

11111111.11111111.11111111.00000000

2^8 = 256 / 64 = 4 Teilnetze

- Netzwerkadresse: 192.168.50.0
- Präfix: /26
- 1. Host: 192.168.50.1
- letzter Host: 192.168.50.62
- Broadcast: 192.168.50.63

**Entwicklung**

2^8 = 256 / 32 = 8 Teilnetze

- Netzwerkadresse: 192.168.50.64
- Präfix: /27
- 1. Host: 192.168.50.65
- letzter Host: 192.168.50.94
- Broadcast: 192.168.50.95

**Support**

2^8 = 256 / 16 = 16 Teilnetze

- Netzwerkadresse: 192.168.50.96
- Präfix: /28
- 1. Host: 192.168.50.97
- letzter Host: 192.168.50.110
- Broadcast: 192.168.50.111

---

![[Pasted image 20260910210503.png]]

Die Teilnetze müssen **variabel** (VLSM) vergeben werden — jede Abteilung bekommt nur so viel Platz, wie sie braucht (plus minimales Reserve).

**Aufgaben:**  
a) Bestimmen Sie für jede Abteilung die passende Subnetzmaske.  
b) Vergeben Sie die Teilnetze **in absteigender Reihenfolge** (größte zuerst), beginnend bei 10.10.0.0. Geben Sie für jede Abteilung Netzadresse, nutzbaren Host-Bereich und Broadcast-Adresse an.  
c) Wie viele Adressen bleiben am Ende ungenutzt übrig?

a)
A /23
B /25
C /26
D /27
E /28

b) 10.10.0.0
	11111111.11111111.11111100.00000000

A /23
2^10 = 1024 / 512 = 2 Teilnetze
Netzadresse: 10.10.0.0
Host-Bereich: 10.10.0.1 - 10.10.1.254
Broadcast: 10.10.1.255
ungenutzt übrig: 510 - 300 = 210

B /25
2^7 = 128 / 128 = 1 Teilnetz
Netzadresse: 10.10.2.0
Host-Bereich: 10.10.2.1 - 10.10.2.126
Broadcast: 10.10.2.127
ungenutzt übrig: 126 - 120 = 6

C /26
2^6 = 64 / 64 = 1 Teilnetz
Netzadresse: 10.10.2.128
Host-Bereich: 10.10.2.129 - 10.10.2.190
Broadcast: 10.10.2.191
ungenutzt übrig: 62 - 60 = 2

D /27
2^5 = 32 / 32 = 1 Teilnetz
Netzadresse: 10.10.2.192
Host-Bereich: 10.10.2.193 - 10.10.2.222
Broadcast: 10.10.2.223
ungenutzt übrig: 30 - 25 = 5

E /28
2^4 = 16 / 16 = 1 Teilnetz
Netzadresse: 10.10.2.224
Host-Bereich: 10.10.2.225 - 10.10.2.238
Broadcast: 10.10.2.239
ungenutzt übrig: 14 - 10 = 4

c)
/22 → 2 ^10 = 1024

1024 / 256 (Oktett) = 4

1. 10.10.0.0 - 10.10.0.255 (genutzt)
2. bis 10.10.1.255 (genutzt)
3. bis 10.10.2.255 (bis 10.10.2.239 genutzt) bleiben 16
4. bis 10.10.3.255 (ungenutzt) bleiben 256

c) 16 + 256 = 272 Adressen


