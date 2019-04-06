Sinatra on Uberspace
####################

<markdown>
Ich mache ja gerne Sachen mit Ruby. Unter anderem auch mit [sinatra](http://www.sinatrarb.com/),
die schlanke Alternative zu [ruby on rails](http://rubyonrails.org/). Lokal rennt das auch ganz gut,
was mich aber nun interessiert hat wie bekomme ich das Teil nun auf meinen [uberspace](https://uberspace.de/)?

Also als erster Schritt mal die Doku durchwühlt (Habt ihr gewusst das Uberspace eine [super Doku](https://uberspace.de/dokuwiki/) hat? sic!). Als Basis habe ich den Artikel zur Einrichtung von [Rails](https://uberspace.de/dokuwiki/cool:rails) genommen. (Es kann auch nicht schaden vorher den [Ruby](https://uberspace.de/dokuwiki/development:ruby) Eintrag zu lesen)

Als erstes richtet man sich nun das aktuelle Ruby ein (ich entnehme das mal der oben genannten Doku)
</markdown>
<code>[helga@helium ~]$ cat <<'__EOF__' >> ~/.bash_profile
export PATH=/package/host/localhost/ruby-1.9.3/bin:$PATH
export PATH=$HOME/.gem/ruby/1.9.1/bin:$PATH
__EOF__

[helga@helium ~]$ . ~/.bash_profile
</code>
<markdown>
Ich bleibe auch dabei den Nutzernamen helga und den Server helium zu nehmen. Das müsst Ihr dann entsprechend austauschen.
Wir werden ein simple Hello World Datei installieren, weil ich denke daran wird das Prinzip auch verstanden.

</markdown>
<code>[helga@helium ~]$ mkdir -p ~/web
[helga@helium ~]$ cat <<'__EOF__' > ~/web/hello.rb
require 'sinatra'

get '/' do
  "Hello World!"
end
__EOF__
</code>
<markdown>

Damit ist unser kleines Hello World File unter ~/web/hello.rb zu finden. Leider reicht das noch nicht, wir brauchen noch eine config file damit nachher der Server auch weiß wo er suchen soll und welches Projekt laufen muss. Das `./hello` muss durch den Dateinamen ersetzt werden der gewünscht ist ohne das .rb am Ende. In unserem Fall heißt die ausführende Datei hello.rb.

</markdown>
<code>[helga@helium ~]$ cat <<'__EOF__' > ~/web/config.ru
require './hello'
run Sinatra::Application
__EOF__
</code>
<markdown>
Jetzt müssen wir nur noch die passenden Gems installieren. Einfacher geht das wenn eine Gemfile vorliegt und man es mit bundler installieren kann. Denkt aber daran das ihr noch ein fcgi hinzufügt zu der Datei, ansonsten gehts nicht. Steht aber auch in der guten Doku von uberspace zu rails.

</markdown>
<code>[helga@helium ~]$ gem install --user-install fcgi sinatra
</code>

So jetzt müssen wir nur noch den kleinen Sinatra Skript ausführen. Dazu nehmen wir aus dem RubyonRails Eintrag den fcgi-skript und wandeln den leicht ab so das er zu unseren Bedürfnissen passt.

<code>[helga@helium ~]$ cat <<__EOF__ > ~/fcgi-bin/HelloSinatra 
#!/bin/sh

# This is needed to find gems installed with --user-install
export HOME=$HOME

# Include our profile to get Ruby 1.9.3 included in our PATH
. \$HOME/.bash_profile

# This makes Rails/Rack think we're running under FastCGI. WTF?!
# See ~/.gem/ruby/1.9.1/gems/rack-1.2.1/lib/rack/handler.rb
export PHP_FCGI_CHILDREN=1

# Get into the project directory and start the Rails server
cd \$HOME/web
exec rackup
__EOF__
</code>

Danach muss noch mit einem `chmod 755 ~/fcgi-bin/HelloSinatra` die Datei ausführbar gemacht werden, und nun läuft unser kleines HelloWorld Programm. Nun müssen wir noch eine kleine .htaccess file in einen Ordner unserer Wahl werfen. Ich bevorzuge da eine Subdomain, weil das netter aussieht ;)

Dazu habe ich folgende .htaccess
<code>#sinatra app up and running
RewriteEngine on
RewriteRule ^(.*)$ /fcgi-bin/HelloSinatra/$1 [QSA,L]
</code>

Und damit gibt es Sinatra nun auch auf dem Uberspace :)