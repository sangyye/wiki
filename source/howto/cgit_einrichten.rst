cgit einrichten
###############

So da ich das letzte mal mit der Installation von [[http://blog.sangyye.de/2012/04/cgit-on-uberspace/|Cgit auf dem uberspace]] beschäftigt habe, wollte ich diesmal so ein bisschen darauf eingehen was man nun damit machen kann.

Also fangen wir mit der Grundeinrichtung an wir hatten bisher das in unsere cgitrc geschrieben

.. code-block:: console

  virtual-root=/git
  enable-index-links=1
  enable-log-filecount=1
  enable-log-linecount=1
  enable-commit-graph=1
  snapshots=tar.gz tar.bz zip

Das ist Erst mal eine einfache Grundconfig, das `virtual-root` beschreibt zum Beispiel den Pfad in dem cgit auf dem Webserver installiert ist. Daher sollte man diese Einstellung nur anfassen wenn sich der Ort ändert.   
Die anderen Einstellung sind so gut gewählt und sorgen dafür das cgit gut aussieht. So wie bekommen wir nun die Repos da rein?

Erst mal verfrachten wir das Repo auf den uberspace. Also ihr habt das Gitrepo lokal auf euren Rechner liegen und wollt das auf den Server in den Ordner `repos` bekommen den ihr vorher angelegt habt.

.. code-block:: console

  git clone --bare testrepo testrepo.git
  scp -r test testrepo.git $ubernaut.$server.uberspace.de:repos/
  # Nun muss man noch das entfernte repo als neues Ziel eintragen
  cd testrepo
  git remote add origin $ubernaut.$server.uberspace.de:repos/testrepo.git

Dann wäre das erledigt und man muss das Repo nur noch in die cgitrc auf dem uberspace eintragen.

.. code-block:: console

  repo.url=testrepo #Das wird der linkname für cgit z.B. git.domain.tld/testrepo
  repo.path=/home/$ubernaut/repos/testrepo.git/ #Pfad zu repo
  repo.desc=Beschreibung hier #Also das ist doch klar
  repo.owner=test@domain.tld #Eine Kontakt mail

So richtet man einfach und schnell ein Repo zum Webinterface hinzufügen. Das kann man für jedes Repo machen.
Es gibt auch eine Möglichkeit mit `project-list` und `scan-path` leider muss ich sagen rechnet sich das vom Aufwand her kaum und dort muss man die infos oben auch nachtragen, was im Prinzip gleichen Aufwand bedeutet :-)

Wünsche allen viel Spass mit cgit.
