---
title: "Jekyll, gh-pages y Cloud9"
categories:
  - misc
tags:
  - Jekyll
  - Cloud9
  - gh-pages
excerpt: "Probando temas Jekyll en Cloud9"
---

Animado por esta estupenda [Introducción a Jekyll](http://juanjosalvador.es/presentaciones/jekyll.html#/), he estado probando a organizar algo más mis publicaciones. 

El perfil en Twitter es muy poco profesional, mezcla multitudes de temas y , bueno, en 160 caracteres se puede expresar poco. Tengo algunas entradas en [Google Plus](https://plus.google.com/102297748253214407201), la verdad con poco movimiento y me daba mucha pereza escribir en [Linkedin](https://www.linkedin.com), así que buscaba

* Algo más dinámico para mantener listas (por ejemplo de gestores de apis, muy al estilo de lo que hace kinlane)
* Algo que me permitiese editar y ordenar mis notas mentales, en un sitio fácil de editar y modificar (teclas vi en Cloud9, [prose](http://prose.io/) directamente en GitHub)
* Algo que me permitiera automatizar de forma sencilla el publicar en los otros sitios (linkedin, googleplus, twitter, etc.)
* Algo con categorías y etiquetas, para cuando no hable de gestión de APIs (como en este caso)

Como no parece buena idea estar haciendo commit de cada prueba directamente en GitHub, he replicado el entorno para probar aspecto y contenidos en [Cloud9](https://c9.io)

Para ello, me he basado en la siguiente información

* [Setting up Jekyll](https://community.c9.io/t/setting-up-jekyll/1707)
Adapta el funcionamiento a cloud9, pero está pensado para iniciar el proyecto, cuando en mi caso ya tenía un fork del [theme](https://github.com/codinfox/codinfox-lanyon) que iba a usar
* [Run Jekyll in the Terminal](https://gist.github.com/Wasserschlange/77b7189c133c15a1d1befab70dc04a8e)
Buena pista sobre como no tener que recordar el comando de arranque específico para Cloud9
* [Running Jekyll on Cloud9](https://www.jflh.ca/2016-01-18-running-jekyll-on-cloud9)
Aparte del comando de arranque, da un buen truco para tener una configuración local que sobreescribe la principal, de modo que sea fácil probar en Cloud9 y luego llevar a GitHub
* [Setting up your GitHub pages site locally](https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/) with Jekyll
Aquí aprendí a establecer un 'Gemfile' específico para gh-pages y no depender del generado por un comando 'jekyll new'

y llegado a la siguiente configuración y pasos

* Fork de [codinfox-lanyon](https://github.com/codinfox/codinfox-lanyon), una modificación de [lanyon](https://github.com/poole/lanyon) un tema muy popular en Jekyll, pero con soporte a categorías y etiquetas
* Creación de un nuevo espacio de trabajo en Cloud9
  * He partido de una plantilla en blanco, ya que la de Ruby parece cargada para lo que necesito y lleva una configuración de ejecución y un esqueleto de aplicación que no necesito 
* Ejecutar en la consola de Cloud9
{% highlight bash %}
~/workspace $ gem install jekyll bundler
~/workspace $ git clone https://github.com/luzbel/codinfox-lanyon.git
~/workspace $ cd codinfox-lanyon
~/workspace/codinfox-lanyon (dev) $ git checkout gh-pages
~/workspace/codinfox-lanyon (gh-pages) $ cat >> Gemfile
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
[CTRL-D EOF]
~/workspace/codinfox-lanyon (gh-pages) $ bundle install
~/workspace/codinfox-lanyon (gh-pages) $ cat >> _cloud9_config.yml
url: "https://[workspace-name]-[user-name].c9users.io/"
baseurl: ""
[CTRL-D EOF]
~/workspace/codinfox-lanyon (gh-pages) $ bundle exec jekyll serve --port $PORT --host $IP --config _config.yml,_cloud9_config.yml 
{% endhighlight %}


# Variante

Si tienes la cuenta de GitHub asociada a la de Cloud9, puedes crear el nuevo espacio de trabajo desde tu repositorio en GitHub

* Fork de [codinfox-lanyon](https://github.com/codinfox/codinfox-lanyon), una modificación de [lanyon](https://github.com/poole/lanyon) un tema muy popular en Jekyll, pero con soporte a categorías y etiquetas
* Ir a tus repositorios en Cloud9
  * Clonar el repositorio codinfox-lanyon que acabas de crear en GitHub
* Ejecutar en la consola de Cloud9
{% highlight bash %}
~/workspace $ gem install jekyll bundler
~/workspace (master) $ git checkout gh-pages
~/workspace (gh-pages) $ cat >> Gemfile
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
[CTRL-D EOF]
~/workspace (gh-pages) $ bundle install
~/workspace (gh-pages) $ cat >> _cloud9_config.yml
url: "https://[workspace-name]-[user-name].c9users.io/"
baseurl: ""
[CTRL-D EOF]
~/workspace (gh-pages) $ bundle exec jekyll serve --port $PORT --host $IP --config _config.yml,_cloud9_config.yml 
{% endhighlight %}

Siguiendo la recomendación, para no tener que acordarse del comando cada vez, genero una configuración de ejecución en Cloud9

# Puntos pendientes

1. Tengo que resolver que hacer con los ficheros Gemfile, Gemfile.lock y _cloud9_config.yml, ya que todavía dudo de sin incluirlos en el .gitignore o incluirlos en el repositorio.
2. Si se clona directamente la página en el raíz del workspace, Jekyll se encuentra constantemente regenerado los ficheros aunque no se modifique ningún fichero. Quizás sean los temporales de vi.
3. Tener una alternativa local, posiblemente con Docker, para cuando quiera editar sin estar conectado.
4. Valorar si interesa tener [ungit](https://github.com/FredrikNoren/ungit) instalado para no pelearme con la línea de comandos.

