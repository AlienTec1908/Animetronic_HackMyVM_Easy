# Animetronic - HackMyVM Writeup

![Animetronic VM Icon](Animetronic.png)

Dieses Repository enthält das Writeup für die HackMyVM-Maschine "Animetronic" (Schwierigkeitsgrad: Easy), erstellt von DarkSpirit. Ziel war es, initialen Zugriff auf die virtuelle Maschine zu erlangen und die Berechtigungen bis zum Root-Benutzer zu eskalieren.

## VM-Informationen

*   **VM Name:** Animetronic
*   **Plattform:** HackMyVM
*   **Autor der VM:** DarkSpirit
*   **Schwierigkeitsgrad:** Easy
*   **Link zur VM:** [https://hackmyvm.eu/machines/machine.php?vm=Animetronic](https://hackmyvm.eu/machines/machine.php?vm=Animetronic)

## Writeup-Informationen

*   **Autor des Writeups:** Ben C.
*   **Datum des Berichts:** 11. Dezember 2023
*   **Link zum Original-Writeup (GitHub Pages):** [https://alientec1908.github.io/Animetronic_HackMyVM_Easy/](https://alientec1908.github.io/Animetronic_HackMyVM_Easy/)

## Kurzübersicht des Angriffspfads

Der Angriff auf die Animetronic-Maschine umfasste folgende Schritte:

1.  **Reconnaissance:**
    *   Identifizierung der Ziel-IP (`192.168.2.104`) mittels `arp-scan`.
    *   Ein `nmap`-Scan offenbarte offene Ports: SSH (22, OpenSSH 8.9p1) und HTTP (80, Apache 2.4.52).
2.  **Web Enumeration:**
    *   `nikto` identifizierte eine veraltete Apache-Version und fehlende Security-Header.
    *   `dirb` entdeckte das Verzeichnis `/staffpages/`.
    *   Thematische OSINT basierend auf "Five Nights at Freddy's" wurde zur Generierung potenzieller Benutzernamen genutzt.
3.  **Benutzerenumeration und Passwortangriff:**
    *   Mittels `msfconsole` (Modul `auxiliary/scanner/ssh/ssh_enumusers`) wurden die validen SSH-Benutzer `mike` und `abby` identifiziert.
    *   Eine benutzerdefinierte Passwortliste wurde mit `cupp` für den Benutzer 'henry' erstellt (jedoch wurde später ein anderer Benutzer kompromittiert).
    *   Ein Brute-Force-Angriff mit `hydra` auf den SSH-Dienst für den Benutzer `michael` (unter Verwendung einer Passwortliste `michael.txt`) war erfolgreich und ergab das Passwort `leahcim1996`.
4.  **Initial Access:**
    *   Erfolgreicher SSH-Login als Benutzer `michael` mit den gefundenen Zugangsdaten.
5.  **Privilege Escalation:**
    *   Die spezifischen Schritte und die ausgenutzte Schwachstelle für die Privilegieneskalation zum Root-Benutzer wurden im vorliegenden Bericht nicht detailliert dokumentiert, es wurde jedoch vermerkt, dass die Eskalation erfolgreich war.
6.  **Flags:**
    *   Auslesen der User- und Root-Flags.

## Verwendete Tools

*   `arp-scan`
*   `vi`
*   `nmap`
*   `nikto`
*   `dirb`
*   `echo`
*   `Metasploit (msfconsole)`
*   `ssh`
*   `curl`
*   `katana`
*   `cupp`
*   `hydra`
*   `cat`

## Identifizierte Schwachstellen (Zusammenfassung)

*   Veraltete Apache-Version (2.4.52), die potenziell anfällig sein könnte.
*   Fehlende HTTP Security Header (`X-Frame-Options`, `X-Content-Type-Options`).
*   Möglichkeit zur Enumeration valider SSH-Benutzernamen.
*   Schwaches und erratbares SSH-Passwort (`leahcim1996` für `michael`).
*   Eine nicht näher dokumentierte Schwachstelle oder Fehlkonfiguration, die eine Privilegieneskalation zum Root-Benutzer ermöglichte.

## Flags

*   **User Flag (`user.txt`):** `0833990328464efff1de6cd93067cfb7`
*   **Root Flag (`root.txt`):** `153a1b940365f46ebed28d74f142530f280a2c0a`

---

**Wichtiger Hinweis:** Dieses Dokument und die darin enthaltenen Informationen dienen ausschließlich zu Bildungs- und Forschungszwecken im Bereich der Cybersicherheit. Die beschriebenen Techniken und Werkzeuge sollten nur in legalen und autorisierten Umgebungen (z.B. eigenen Testlaboren, CTFs oder mit expliziter Genehmigung) angewendet werden. Das unbefugte Eindringen in fremde Computersysteme ist eine Straftat und kann rechtliche Konsequenzen nach sich ziehen.

---
*Bericht von Ben C. - Cyber Security Reports*
