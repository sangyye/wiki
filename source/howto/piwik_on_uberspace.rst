Piwik on Uberspace
##################

Was ist den eigentlich Piwik?
=============================

Ihr alle kennt sicher Google Analytics und Co.? Piwik ist die Alternative zum selber hosten und damit müssen
keine Daten ins Ausland geschickt werden. Es ist also möglich eine Datenerhebung zur Analyse der Webseite zu machen im Sinne der
Benutzer und des Datenschutzes zu machen. Piwik ist in PHP geschrieben und es gibt viele Plugins für die Integration in verschieden Systeme.

Wie bekomme ich das tolle Teil nun auf meinen Uberspace?
========================================================

Nichts leichter als das =) Und dauert auch nur knapp 5 Minuten.   
Entweder auf der Konsole via ssh
<code>mkdir -p tmp && cd tmp
wget http://piwik.org/latest.zip
unzip latest.zip
mv piwik/ ../html
</code>
Und nun habt ihr unter deine-domain.de/piwik oder eurer uberspace Seite im Unterverzeichniss /piwik (da sogar mit ssl)
Wer lieber seinen sftp Client nutzt, einfach das [[http://piwik.org/latest.zip|latest.zip]] runterladen, entpacken. Und in das Verzeichniss html schieben. Fertig.
Nur noch eben den Link aufrufen und sein piwik mit den mysql daten füttern und grundeinrichten. Das wars.

Ja, das ist ja alles toll, nur wie bekommen ich das nun in mein Wordpress?
==========================================================================

Das ist sogar sehr einfach, einfach das [[http://wordpress.org/extend/plugins/wp-piwik/|WP-piwik Plugin]] installieren.
Dann sich bei seinem piwik einloggen und oben am Rand gibt es einen Menupunkt API. Dort bekommt man den Authkey den man
zusammen mit der URL (deine-domain.de/piwik) in die Einstellungen des wp-piwik eintragen. Dann rennt das Teil und generiert Statistiken.

So ich hoffe das Hilft und eine kleine Einführungen in den Funktionsumfang von Piwik folgt.