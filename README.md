# mutt-wizard

https://muttwizard.com/

Erhalten Sie diese großartigen Funktionen ohne Aufwand:

- Einen voll ausgestatteten und automatisch konfigurierten E-Mail-Client im Terminal mit neomutt
- Offline gespeicherte E-Mails, die es Ihnen ermöglichen:
    * E-Mails zu lesen und zu schreiben, während Sie offline sind
    * Backups zu erstellen
- Stellt ein `mailsync`-Skript bereit, das so oft wie gewünscht ausgeführt werden kann, um E-Mails herunterzuladen/zu synchronisieren und optional bei Erhalt neuer E-Mails zu benachrichtigen.

Dieser Wizard:

- Ermittelt die IMAP- und SMTP-Server und -Ports Ihres E-Mail-Servers
- Erstellt Dot-Files für `neomutt`, `isync` und `msmtp`, die für Ihre E-Mail-Adresse geeignet sind
- Verschlüsselt und speichert Ihr Passwort lokal für einfachen Fernzugriff, nur mit Ihrem GPG-Schlüssel zugänglich
- Verwaltet automatisch bis zu neun separate E-Mail-Konten
- Erstellt automatisch Bindungen zum Wechseln zwischen Konten oder Postfächern
- Bietet sinnvolle Standardeinstellungen und ein attraktives Erscheinungsbild für den neomutt E-Mail-Client
- Wenn mutt-wizard die IMAP/SMTP-Informationen Ihres Servers nicht standardmäßig kennt, werden Sie danach gefragt und die Informationen an allen richtigen Stellen eingetragen.

## Installation

#### Abhängigkeiten

- `neomutt` - der E-Mail-Client. (Wenn Sie Gentoo GNU/Linux verwenden, benötigen Sie das `sasl` Use-Flag aktiviert)
- `curl` - testet Verbindungen (bei der Installation erforderlich).
- `isync` - lädt E-Mails herunter und synchronisiert sie (erforderlich, wenn IMAP-E-Mails lokal gespeichert werden).
- `msmtp` - sendet die E-Mails.
- `pass` - verschlüsselt Passwörter sicher (bei der Installation erforderlich).
- `ca-certificates` - für SSL erforderlich. Wahrscheinlich bereits installiert.
- `gettext` - schreibt Konfigurationsdateien. Wahrscheinlich bereits installiert.

**Hinweis**: Bei langsam aktualisierten Distributionen wie Ubuntu, Debian oder Mint können Fehler auftreten. Wenn Sie Fehler in `neomutt` erhalten, installieren Sie die neueste Version manuell oder entfernen Sie die problematischen Zeilen in der Konfiguration in `/usr/share/mutt-wizard/mutt-wizard.muttrc` manuell.

```bash
git clone https://github.com/LukeSmithxyz/mutt-wizard
cd mutt-wizard
sudo make install
```

Benutzer von Arch-basierten Distributionen können auch die aktuelle mutt-wizard-Version aus dem AUR als [mutt-wizard](https://aur.archlinux.org/packages/mutt-wizard/) oder den Github-Master-Branch als [mutt-wizard-git](https://aur.archlinux.org/packages/mutt-wizard-git/) installieren.

### Optionale Abhängigkeiten

- `pam-gnupg` - Meldet Sie beim Login automatisch bei Ihrem GPG-Schlüssel an, sodass Sie nach der Anmeldung an Ihrem System nie Ihr Passwort eingeben müssen. Prüfen Sie das Repository und die Anweisungen [hier](https://github.com/cruegge/pam-gnupg).
- `lynx` - HTML-E-Mails in neomutt anzeigen.
- `notmuch` - E-Mails indizieren und durchsuchen. Installieren Sie es und führen Sie `notmuch setup` aus, geben Sie an, dass sich Ihre E-Mails in `~/.local/share/mail/` befinden (obwohl `mw` dies automatisch tut, wenn Sie notmuch noch nicht eingerichtet haben). Sie können es in mutt mit <kbd>Strg-f</kbd> ausführen. Führen Sie `notmuch new` aus, um neue E-Mails zu verarbeiten.
- `abook` - ein terminalbasiertes Adressbuch. Durch Drücken der Tab-Taste während der Eingabe einer Adresse zum Senden von E-Mails werden Kontakte vorgeschlagen, die sich in Ihrem abook befinden.
- `urlview` - gibt URLs in E-Mails an den Browser weiter.
- `cronie` - (oder ein anderer großer Cronjob-Manager) zum Einrichten der automatischen E-Mail-Synchronisation.
- `mpop` - Wenn Sie das POP-Protokoll anstelle von IMAP verwenden möchten.

## Verwendung

Der mutt-wizard wird über den Befehl `mw` ausgeführt. Nach Abschluss der Einrichtung verwenden Sie `neomutt`, um auf Ihre E-Mails zuzugreifen.

- `mw -a you@email.com` -- ein neues E-Mail-Konto hinzufügen
- `mw -l` -- vorhandene Konten auflisten
- `mw -d` -- ein zu löschendes Konto auswählen
- `mw -D your@email.com` -- Kontoeinstellungen ohne Bestätigung löschen
- `mw -t 30` -- automatische Mailsync alle 30 Minuten umschalten
- `mw -T` -- Mailsync umschalten ohne Minutenangabe (Standard ist 10)
- `mw -r` -- Kontokürzelnummern neu ordnen
- `pass edit mw-your@email.com` -- das Passwort eines Kontos überarbeiten
- `mailsync` -- alle konfigurierten E-Mail-Konten synchronisieren. Gibt auch Benachrichtigungen über neue E-Mails und indiziert neue E-Mails stillschweigend mit notmuch.
- `mailsync your@email.com` -- ein bestimmtes (oder mehrere) E-Mail-Konto(en) synchronisieren.

### Optionen beim Hinzufügen eines Kontos

#### Bereitstellung von Argumenten

- `-u` -- Geben Sie einen Benutzernamen für das Konto an, wenn er sich von der E-Mail-Adresse unterscheidet.
- `-n` -- Ein echter Name, der vom Konto verwendet werden soll. In Anführungszeichen setzen, wenn es mehrere Wörter sind.
- `-i` -- IMAP-Server-Adresse
- `-I` -- IMAP-Server-Port (ansonsten wird 993 angenommen)
- `-s` -- SMTP-Server-Adresse
- `-S` -- SMTP-Server-Port (ansonsten wird 465 angenommen)
- `-m` -- Maximale Anzahl von E-Mails, die offline gehalten werden sollen. Standardmäßig gibt es kein Maximum.
- `-x` -- Kontopasswort. Andernfalls werden Sie danach gefragt.

#### Allgemeine Einstellungen

- `-f` -- Postfachnamen annehmen und die Kontokonfiguration erzwingen, ohne überhaupt online zu gehen.
- `-o` -- Mutt für ein Konto konfigurieren, aber keine E-Mails offline halten.
- `-p` -- POP-Protokoll anstelle von IMAP verwenden (erfordert die Installation von `mpop`).
- `mailsync` gibt standardmäßig visuelle Nachrichten über neue E-Mails aus. Oder setzen Sie `MAILSYNC_MUTE=1` als Umgebungsvariable, wenn Sie diese nicht haben möchten.

## Neomutt Benutzeroberfläche

Um Ihnen eine Vorstellung von der Benutzeroberfläche zu geben, hier eine Idee:

- <kbd>m</kbd> - E-Mail senden (verwendet Ihren Standard-`$EDITOR` zum Schreiben)
- <kbd>j</kbd>/<kbd>k</kbd> und <kbd>d</kbd>/<kbd>u</kbd> - vim-ähnliche Bindungen zum Auf- und Abwärtsbewegen (oder <kbd>d</kbd>/<kbd>u</kbd> um eine Seite nach unten/oben zu gehen).
- <kbd>l</kbd> - E-Mail öffnen, oder Anhangsseite oder Anhang
- <kbd>h</kbd> - das Gegenteil von <kbd>l</kbd>
- <kbd>r</kbd>/<kbd>R</kbd> - auf hervorgehobene E-Mail antworten/allen antworten
- <kbd>s</kbd> - ausgewählte E-Mail oder ausgewählten Anhang speichern
- <kbd>gs</kbd>,<kbd>gi</kbd>,<kbd>ga</kbd>,<kbd>gd</kbd>,<kbd>gS</kbd> - Drücken Sie <kbd>g</kbd> gefolgt von einem anderen Buchstaben, um das Postfach zu wechseln: <kbd>s</kbd>ent (gesendet), <kbd>i</kbd>nbox (Posteingang), <kbd>a</kbd>rchive (Archiv), <kbd>d</kbd>rafts (Entwürfe), <kbd>S</kbd>pam, usw.
- <kbd>M</kbd> und <kbd>C</kbd> - Für <kbd>M</kbd>ove (Verschieben) und <kbd>C</kbd>opy (Kopieren): Folgen Sie ihnen mit einem der obigen Postfachbuchstaben, d.h. <kbd>MS</kbd> bedeutet "in Spam verschieben".
- <kbd>i#</kbd> - Drücken Sie <kbd>i</kbd> gefolgt von einer Zahl 1-9, um zu einem anderen Konto zu wechseln. Wenn Sie 9 Konten über mutt-wizard hinzufügen, wird jedem eine Nummer zugewiesen.
- <kbd>a</kbd> um Adresse/Person zu abook hinzuzufügen und <kbd>Tab</kbd> während der Adresseingabe, um eine aus abook zu vervollständigen.
- <kbd>?</kbd> - alle Tastaturkürzel anzeigen
- <kbd>Strg-j</kbd>/<kbd>Strg-k</kbd> - in der Seitenleiste auf- und abwärts bewegen, <kbd>Strg-o</kbd> öffnet das Postfach.
- <kbd>Strg-b</kbd> - ein Menü öffnen, um eine URL auszuwählen, die Sie in Ihrem Browser öffnen möchten.
- <kbd>p</kbd> - Ihre Nachricht verschlüsseln/signieren (in der Verfassungsansicht, bevor Sie die E-Mail senden).

## Zusätzliche Funktionalität

- `pam-gnupg` - Meldet Sie beim Login automatisch bei Ihrem GPG-Schlüssel an, sodass Sie nach der Anmeldung an Ihrem System nie Ihr Passwort eingeben müssen. Prüfen Sie das Repository und die Anweisungen [hier](https://github.com/cruegge/pam-gnupg).
- `lynx` - HTML-E-Mails in neomutt anzeigen.
- `notmuch` - E-Mails indizieren und durchsuchen. Installieren Sie es und führen Sie `notmuch setup` aus, geben Sie an, dass sich Ihre E-Mails in `~/.local/share/mail/` befinden (obwohl `mw` dies automatisch tut, wenn Sie notmuch noch nicht eingerichtet haben). Sie können es in mutt mit <kbd>Strg-f</kbd> ausführen. Führen Sie `notmuch new` aus, um neue E-Mails zu verarbeiten.
- `abook` - Ein terminalbasiertes Adressbuch. Durch Drücken der Tab-Taste während der Eingabe einer Adresse zum Senden von E-Mails werden Kontakte vorgeschlagen, die sich in Ihrem abook befinden.
- `urlview` - Gibt URLs in einer E-Mail an Ihren Browser weiter.

## Neue Funktionen und Verbesserungen seit der ursprünglichen Veröffentlichung

- `mw` ist jetzt über Befehlszeilenoptionen skriptfähig und kann erfolgreich ohne jegliche Interaktion ausgeführt werden, was es möglich macht, es in einem Skript einzusetzen.
- `isync`/`mbsync` hat `offlineimap` als Backend ersetzt. Offlineimap war fehleranfällig, aufgebläht, verwendete veraltete Python 2-Module und erforderte separate Schritte zur Installation des Systems.
- `mw` ist jetzt ein installiertes Programm, anstatt nur ein Skript, das in Ihrem mutt-Ordner aufbewahrt werden musste.
- `dialog` wird nicht mehr verwendet und die Schnittstelle besteht einfach aus Textbefehlen.
- Mehr automatisch generierte Tastenkombinationen, die ein schnelles Verschieben und Kopieren von E-Mails zwischen Postfächern ermöglichen.
- Elegantere Behandlung von Anhängen. Bild-/Video-/PDF-Anhänge ohne Abhängigkeit von der neomutt-Instanz.
- abook-Integration standardmäßig.
- Die unübersichtlichen Vorlagendateien und anderen Verzeichnisse wurden verschoben oder entfernt, was zu einem sauberen Konfigurationsordner führt.
- msmtp-Konfigurationen wurden nach `~/.config/` verschoben und der Standard-E-Mail-Speicherort wurde nach `~/.local/share/mail/` verschoben, was die Unordnung in `~` reduziert.
- `pass` wird als Passwort-Manager verwendet, anstatt Passwörter separat zu speichern.
- Das Skript ist POSIX sh-konform.
- Fehlerbehandlung für die vielen Leute, die Anweisungen nicht lesen oder befolgen. Generell weniger Fehler.
- Hinzufügen eines Handbuchs `man mw`
- Verarbeitet jetzt das POP-Protokoll über `mpop` für diejenigen, die es bevorzugen (fügen Sie ein Konto mit der Option `-p` hinzu). POP-Konfigurationen werden immer noch automatisch generiert.

## Helfen Sie dem Projekt!

- Testen Sie mutt-wizard auf ungewöhnlichen Maschinen und mit ungewöhnlichen E-Mail-Adressen und melden Sie alle Fehler.
- Öffnen Sie einen PR, um neue Serverinformationen in `domains.csv` hinzuzufügen, damit deren Benutzer mutt-wizard einfacher verwenden können.
- Wenn sonst nichts, spenden Sie:
	- XMR: `8AzeWXhJvYJ1VeENHcNXCR1dLMgDALreZ1BdooZVjRKndv6myr3t1ue6C4ML2an5fWSpcP1sTDA9nKUMevkukDXG6chRjNv`
	- BTC: `bc1qacqfp36ffv9mafechmvk8f6r8qy4tual6rcm9p`

## Details für Bastler

- Die wichtigen `mutt`/`neomutt`-Dateien befinden sich in `~/.config/mutt/`.
- Legen Sie alle globalen Einstellungen, die Sie möchten, in `muttrc` fest. mutt-wizard wird einige Zeilen zu dieser Datei hinzufügen, die Sie nicht entfernen sollten, es sei denn, Sie wissen, was Sie tun, aber Sie können sie über Ihre Konfigurationszeilen nach oben/unten verschieben, wenn nötig. Wenn Sie Bindungskonflikte in mutt erhalten, müssen Sie dies möglicherweise tun.
- Jedes der von mutt-wizard generierten Konten wird benutzerdefinierte Einstellungen in einer separaten Datei in `accounts/` haben. Sie können diese frei bearbeiten, wenn Sie mit den Einstellungen eines bestimmten Kontos experimentieren möchten.
- In `/usr/share/mutt-wizard` befinden sich mehrere globale Konfigurationsdateien, einschließlich der Standardeinstellungen von `mutt-wizard`. Sie können diese in Ihrer `muttrc` überschreiben, wenn Sie möchten.

## Achten Sie auf diese Dinge

- Gmail-Konten müssen ein [App-Passwort](https://support.google.com/accounts/answer/185833?hl=de) erstellen, um mit "weniger sicheren" Anwendungen zu arbeiten. Dieses Passwort ist einmalig (d.h. für die Einrichtung) und wird lokal gespeichert und verschlüsselt. Die Aktivierung von Drittanbieteranwendungen erfordert das Ausschalten der Zwei-Faktor-Authentifizierung, und dies wird das umgehen. Möglicherweise müssen Sie auch manuell "IMAP aktivieren" in den Einstellungen.
  Um ein App-Passwort für Ihr Google-Konto zu erstellen, können Sie direkt die [App-Passwörter](https://myaccount.google.com/apppasswords) Seite in Ihren Google-Kontoeinstellungen besuchen.
- Wenn Sie eine Universitäts-E-Mail oder eine von Unternehmen gehostete E-Mail für die Arbeit haben, könnte es andere Hürden oder Zwei-Faktor-Authentifizierung geben, die Sie überwinden müssen. Einige werden zum Beispiel von Ihnen verlangen, ein separates IMAP-Passwort zu erstellen, usw.
- `isync` ist nicht vollständig UTF-8-kompatibel, daher können nicht-lateinische Zeichen möglicherweise verstümmelt sein (obwohl die Synchronisation erfolgreich sein sollte). `mw` wird auch keine automatischen Postfach-Verknüpfungen erstellen, da es nach englischen Postfachnamen sucht. Ich empfehle Ihnen dringend, die Sprache Ihrer E-Mails auf Ihrem E-Mail-Server auf Englisch einzustellen, um diese Probleme zu vermeiden.

## Lizenz

mutt-wizard ist freie/libre Software. Dieses Programm wird unter der GPLv3-Lizenz veröffentlicht, die Sie in der Datei [LICENSE](LICENSE) finden können.
```
