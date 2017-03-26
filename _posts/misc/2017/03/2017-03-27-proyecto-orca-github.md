---
title: "Migración proyecto [ORCA](http://es.tldp.org/ORCA/glosario.html) a GitHub"
categories:
  - misc
tags:
  - ORCA
  - GitHub
  - HackLabAlmeria
  
excerpt: "Versión de ORCA, un glosario de términos informáticos en inglés mantenido por la comunidad, con yaml en gh-pages"
---

Leyendo el [foro del HackLabAlmeria](https://foro.hacklabalmeria.net/t/wikipedia-vs-rae/8257/4) me entero del "injustamente desconocido y desgraciadamente infrautilizado" proyecto ORCA, en el que la comunidad mantiene un [glosario](http://es.tldp.org/ORCA/glosario.html) de terminos informáticos en inglés. Pensando en aplicarlo a mis entradas en la bitácora, de modo que junto a cada término en inglés apareciese su equivalente en castellano, pruebo a realizar una versión  para gh-pages en el que el glosario se mantenga en formato YAML. Presentaré la propuesta al HackLabAlmeria, por si deciden mantener el glosario en su página. 

## Pasos

### Crear un espacio de trabajo en Cloud9 con mi copia de la web del HackLab, de modo que pueda clonarla cada vez que quiera realizar algún experimento

* Crear un nuevo *workspace* en Cloud9 clonando https://github.com/luzbel/hacklab-almeria.github.io.git
* Sigo https://help.github.com/articles/configuring-a-remote-for-a-fork/ para sincronizar mi *fork* con el repositorio principal

```
luzbel:~/workspace (master) $ git remote add upstream https://github.com/HackLab-Almeria/hacklab-almeria.github.io.git
luzbel:~/workspace (master) $ git remote -v
origin  https://github.com/luzbel/hacklab-almeria.github.io.git (fetch)
origin  https://github.com/luzbel/hacklab-almeria.github.io.git (push)
upstream        https://github.com/HackLab-Almeria/hacklab-almeria.github.io.git (fetch)
upstream        https://github.com/HackLab-Almeria/hacklab-almeria.github.io.git (push)
``` 

* Sigo https://help.github.com/articles/syncing-a-fork/ para actualizar mi copia desde mi último PR

```
luzbel:~/workspace (master) $ git fetch upstream
remote: Counting objects: 129, done.
remote: Total 129 (delta 34), reused 34 (delta 34), pack-reused 95
Receiving objects: 100% (129/129), 819.15 KiB | 0 bytes/s, done.
Resolving deltas: 100% (82/82), completed with 15 local objects.
From https://github.com/HackLab-Almeria/hacklab-almeria.github.io
 * [new branch]      master     -> upstream/master
luzbel:~/workspace (master) $ git checkout master
Already on 'master'
Your branch is up-to-date with 'origin/master'.
luzbel:~/workspace (master) $ git merge upstream/master
Updating feb4ef4..9efed4b
Fast-forward
 _layouts/default.html                                         |  13 ++++++++++++-
 _posts/2016-04-27-feria-de-ideas-ual.md                       |   2 +-
 _posts/2016-12-10-iii-jornadas-hacklab.html                   |  28 +++++++++++++++++-----------
 _posts/2017-01-02-encuentro-propositos-2017.md                |  26 ++++++++++++++++++++++++++
 _posts/2017-01-19-reunion-pymiento.md                         |  47 +++++++++++++++++++++++++++++++++++++++++++++++
 _posts/2017-01-26-taller-hardware-abierto.md                  |  76 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 _posts/2017-02-23-taller-hardware-abierto.md                  |  73 +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 _posts/2017-02-24-FPGAs.md                                    |  43 +++++++++++++++++++++++++++++++++++++++++++
 _posts/2017-03-10-i-jornadas-mujeres-y-tecnologia.md          |  44 ++++++++++++++++++++++++++++++++++++++++++++
 aviso/index.md                                                |   8 ++++----
 css/style.css                                                 |   5 ++++-
 index.html                                                    |  15 ---------------
 recursos/20161210/jornadas_hacklab_2016_by_roxyhana-800px.png | Bin 261463 -> 273077 bytes
 recursos/20161210/jornadas_hacklab_2016_by_roxyhana.png       | Bin 1236021 -> 1325598 bytes
 recursos/2017-01-19/pymiento-black.png                        | Bin 0 -> 156069 bytes
 recursos/2017-03-10/cartel.jpg                                | Bin 0 -> 124884 bytes
 16 files changed, 347 insertions(+), 33 deletions(-)
 create mode 100644 _posts/2017-01-02-encuentro-propositos-2017.md
 create mode 100644 _posts/2017-01-19-reunion-pymiento.md
 create mode 100644 _posts/2017-01-26-taller-hardware-abierto.md
 create mode 100644 _posts/2017-02-23-taller-hardware-abierto.md
 create mode 100644 _posts/2017-02-24-FPGAs.md
 create mode 100644 _posts/2017-03-10-i-jornadas-mujeres-y-tecnologia.md
 create mode 100644 recursos/2017-01-19/pymiento-black.png
 create mode 100644 recursos/2017-03-10/cartel.jpg
```

* Repaso mis [notas](https://luzbel.github.io/Cuaderno-de-bitacora/misc/cloud9-ghpages/) de como tener jekyll para gh-pages en Cloud9

```
luzbel:~/workspace (master) $ gem install jekyll bundler
Fetching: public_suffix-2.0.5.gem (100%)
Successfully installed public_suffix-2.0.5
Fetching: addressable-2.5.0.gem (100%)
Successfully installed addressable-2.5.0
Fetching: colorator-1.1.0.gem (100%)
Successfully installed colorator-1.1.0
Fetching: sass-3.4.23.gem (100%)
Successfully installed sass-3.4.23
Fetching: jekyll-sass-converter-1.5.0.gem (100%)
Successfully installed jekyll-sass-converter-1.5.0
Fetching: rb-fsevent-0.9.8.gem (100%)
Successfully installed rb-fsevent-0.9.8
Fetching: ffi-1.9.18.gem (100%)
Building native extensions.  This could take a while...
Successfully installed ffi-1.9.18
Fetching: rb-inotify-0.9.8.gem (100%)
Successfully installed rb-inotify-0.9.8
Fetching: listen-3.0.8.gem (100%)
Successfully installed listen-3.0.8
Fetching: jekyll-watch-1.5.0.gem (100%)
Successfully installed jekyll-watch-1.5.0
Fetching: kramdown-1.13.2.gem (100%)
Successfully installed kramdown-1.13.2
Fetching: liquid-3.0.6.gem (100%)
Successfully installed liquid-3.0.6
Fetching: mercenary-0.3.6.gem (100%)
Successfully installed mercenary-0.3.6
Fetching: forwardable-extended-2.6.0.gem (100%)
Successfully installed forwardable-extended-2.6.0
Fetching: pathutil-0.14.0.gem (100%)
Successfully installed pathutil-0.14.0
Fetching: rouge-1.11.1.gem (100%)
Successfully installed rouge-1.11.1
Fetching: safe_yaml-1.0.4.gem (100%)
Successfully installed safe_yaml-1.0.4
Fetching: jekyll-3.4.3.gem (100%)
Successfully installed jekyll-3.4.3
Fetching: bundler-1.14.6.gem (100%)
Successfully installed bundler-1.14.6
19 gems installed
luzbel:~/workspace (master) $ cat >> Gemfile
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
[CTRL-D EOF]
luzbel:~/workspace (master) $ more Gemfile 
source 'https://rubygems.org'
gem 'github-pages', group: :jekyll_plugins
luzbel:~/workspace (master) $ bundle install
Fetching gem metadata from https://rubygems.org/..............
Fetching version metadata from https://rubygems.org/...
Fetching dependency metadata from https://rubygems.org/..
Resolving dependencies...
Installing i18n 0.8.1
Installing json 1.8.6 with native extensions
Installing minitest 5.10.1
Installing thread_safe 0.3.6
Using public_suffix 2.0.5
Installing coffee-script-source 1.12.2
Installing execjs 2.7.0
Using colorator 1.1.0
Using ffi 1.9.18
Installing multipart-post 2.0.0
Using forwardable-extended 2.6.0
Installing gemoji 3.0.0
Installing net-dns 0.8.0
Using sass 3.4.23
Using rb-fsevent 0.9.8
Using kramdown 1.13.2
Using liquid 3.0.6
Using mercenary 0.3.6
Using rouge 1.11.1
Using safe_yaml 1.0.4
Installing mini_portile2 2.1.0
Installing jekyll-paginate 1.1.0
Installing jekyll-swiss 0.4.0
Installing minima 2.0.0
Installing unicode-display_width 1.1.3
Using bundler 1.14.6
Installing tzinfo 1.2.2
Using addressable 2.5.0
Installing coffee-script 2.4.1
Installing ethon 0.10.1
Using rb-inotify 0.9.8
Installing faraday 0.11.0
Using pathutil 0.14.0
Using jekyll-sass-converter 1.5.0
Installing nokogiri 1.6.8.1 with native extensions
Installing terminal-table 1.7.3
Installing activesupport 4.2.7
Installing jekyll-coffeescript 1.0.1
Installing typhoeus 0.8.0
Installing listen 3.0.6
Installing sawyer 0.8.1
Installing html-pipeline 2.5.0
Using jekyll-watch 1.5.0
Installing octokit 4.6.2
Installing jekyll 3.4.2
Installing github-pages-health-check 1.3.3
Installing jekyll-gist 1.4.0
Installing jekyll-avatar 0.4.2
Installing jekyll-default-layout 0.1.4
Installing jekyll-feed 0.9.1
Installing jekyll-github-metadata 2.3.1
Installing jekyll-mentions 1.2.0
Installing jekyll-optional-front-matter 0.1.2
Installing jekyll-readme-index 0.0.4
Installing jekyll-redirect-from 0.12.1
Installing jekyll-relative-links 0.3.0
Installing jekyll-seo-tag 2.1.0
Installing jekyll-sitemap 1.0.0
Installing jekyll-theme-architect 0.0.3
Installing jekyll-theme-cayman 0.0.3
Installing jekyll-theme-dinky 0.0.3
Installing jekyll-theme-hacker 0.0.3
Installing jekyll-theme-leap-day 0.0.3
Installing jekyll-theme-merlot 0.0.3
Installing jekyll-theme-midnight 0.0.3
Installing jekyll-theme-minimal 0.0.3
Installing jekyll-theme-modernist 0.0.3
Installing jekyll-theme-primer 0.1.7
Installing jekyll-theme-slate 0.0.3
Installing jekyll-theme-tactile 0.0.3
Installing jekyll-theme-time-machine 0.0.3
Installing jekyll-titles-from-headings 0.1.4
Installing jemoji 0.8.0
Installing github-pages 128
Bundle complete! 1 Gemfile dependency, 74 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.
Post-install message from html-pipeline:
-------------------------------------------------
Thank you for installing html-pipeline!
You must bundle Filter gem dependencies.
See html-pipeline README.md for more details.
https://github.com/jch/html-pipeline#dependencies
-------------------------------------------------
Post-install message from minima:

----------------------------------------------
Thank you for installing minima 2.0!

Minima 2.0 comes with a breaking change that
renders '<your-site>/css/main.scss' redundant.
That file is now bundled with this gem as
'<minima>/assets/main.scss'.

More Information:
https://github.com/jekyll/minima#customization
----------------------------------------------

luzbel:~/workspace (master) $ cat > _cloud9_config.yml
url: "https://hacklab-almeria-github-io-luzbel.c9users.io"
baseurl: ""
repository: luzbel/hacklab-almeria.github.io
luzbel:~/workspace (master) $ more _cloud9_config.yml 
url: "https://hacklab-almeria-github-io-luzbel.c9users.io"
baseurl: ""
repository: luzbel/hacklab-almeria.github.io
```

* A destacar que en _cloud9_config.yml la url va con todo separado por guiones y que hace falta el valor de repository en las nuevas versiones de Jekyll
* Para ejecutar, crear una nueva configuración con:

```
bundle exec jekyll serve --port $PORT --host $IP --config _config.yml,_cloud9_config.yml
```

* Y con la variable de entorno JEKYLL_ENV con valor *production*


### Clonar el espacio de trabajo base para experimentar en una copia y probar a migrar el proyecto ORCA

* Clonar el repositorio en Cloud9
* Editar el fichero _cloud9_config.yml para actualizar la url con la del nuevo repositorio
* Crear una rama para hacer la prueba

```
luzbel:~/workspace (master) $ git checkout -b ORCA
Switched to a new branch 'ORCA'
``` 

* creo el fichero de datos, hago un index.md de prueba

[Ver](https://github.com/luzbel/hacklab-almeria.github.io/commit/778a604f7122dd6d7c8cb07d9ca4522617c3cbee) los cambios

* Subo los cambios a mi copia de la página del HackLab

```
git push origin ORCA
```

* Creo el siguiente [pull-request](https://github.com/HackLab-Almeria/hacklab-almeria.github.io/pull/143) (¿o debería decir "petición de subida"?)

## Temas pendientes

* He partido de la "Versión 2.1.0, 21 de mayo de 2002" de http://es.tldp.org/ORCA/glosario.html , cuando existe la "Versión 2.2.0, 21 de mayo de 2005" de http://quark.fe.up.pt/orca/pub-es/glosario.html
* Revisar, hay gazapos e incluso términos en español. Por ejemplo, encriptar ya existe
* Añadir
  * serverless
  * pull request
  * on-premise
* los verbos se deberían marcar con un atributo en el yaml y no con (v)
* enlaces cuando pone véase
* texto flotante en Jekyll cuando pones término en inglés
  * clonar en mi bitácora
* el formato de la tabla del índice, salen celdas naranjas para la letra A y N 
* añadir un menú o forma de llegar a la página desde la navegación de la web del HackLab
* pasar estos puntos a *issues* de GitHub