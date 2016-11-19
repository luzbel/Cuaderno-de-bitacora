---
title: "Jekyll, theme Minimal Mistakes y Cloud9"
categories:
  - misc
tags:
  - Jekyll
  - Cloud9
  - gh-pages
excerpt: "Probando temas minimal mistakes de Jekyll en Cloud9"
---

Después de probar con codinfox-lanyon, ahora pruebo con otro tema, que parece más elaborado

* Fork de [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes), un tema con licencia MIT y muy buena pinta
* Ir a tus repositorios en Cloud9
  * Clonar el repositorio minimal-mistakes que acabas de crear en GitHub
* Ejecutar en la consola de Cloud9
{% highlight bash %}
~/workspace $ gem install jekyll bundler
~/workspace (master) $ git checkout gh-pages
~/workspace (gh-pages) $ bundle install
~/workspace (gh-pages) $ cat >> _cloud9_config.yml
url: "https://[workspace-name]-[user-name].c9users.io/"
baseurl: ""
[CTRL-D EOF]
~/workspace (gh-pages) $ JEKYLL_ENV=production bundle exec jekyll serve --port $PORT --host $IP --config _config.yml,_cloud9_config.yml 
{% endhighlight %}
  * A diferencia del otro tema, este viene previamente empaquetado
  * En las dependencias usa una versión más nueva de Jekyll (3.3.1), que al ejecutarse dentro de Cloud9 con el host 0.0.0.0, provocaban errores en todos los enlaces. 
    * Inicializando la variable de entorno JEKYLL_ENV a "production" el comportamiento es el correcto
  * Con el Gemfile con los valores indicados en la [guía rápida](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/) de inicio del tema, para cuando se clona de GitHub, los enlaces apuntan a direcciones erróneas (quizás pensando en que el tema lo sueles instalar como página de usuario y no como página de un repositorio). Por cierto, se usa Jekyll 3.3.0 en vez de la 3.3.1 de las otras configuraciones
  * Con el Gemfile recomendado en la guía para cuando se usa su empaquetado, funciona. 
  * Con el Gemfile por defecto al hacer el fork (solo una línea "gemspec"), funciona tanto en Cloud9 como en GitHub, así que dejo este.

Siguiendo la recomendación, para no tener que acordarse del comando cada vez, genero una configuración de ejecución en Cloud9

{% highlight bash %}
JEKYLL_ENV=production bundle exec jekyll serve --port $PORT --host $IP --config _config.yml,_cloud9_config.yml
{% endhighlight %}

# Puntos pendientes

1. Probar con la variable de entorno BUNDLE_GEMFILE, para tener un fichero alternativo para Cloud9 si al final tengo una versión distinta para gh-pages
2. Ya que modifico las instrucciones, quizás esto debería ir en una _page y no en un _post
3. ~~Repetir los pasos, por si funciona por combinación de factores y usando el bundle inicial, no.~~ comprobado
4. Al tratar de hacer un feed específico por canal (twitter,linkedin,g+,...) el XML resultante da problemas en Chrome, si en el campo email, en vez de una dirección valida, ponemos la URL de la web de google antispam
  * Es decir, esto funciona
{% highlight bash %}
  <author>
    <name>{{ site.author.name }}</name>
    <email>pedroparra@fakeserver.com</email>
  </author>
{% endhighlight %}
  * Pero esto no
{% highlight bash %}
 <author>
   <name>{{ site.author.name }}</name>
   <email>"http://www.google.com/recaptcha/mailhide/d?k=01iNfZFgkhPfOwGK2_m0Xrug==&c=vrWlHe-RPOFhSGrpeXA8abLyI-FOT_u1Pf5-GJMM1eeUrrXAj_iHaxd3kxc9Zrjm"</email>
 </author>
{% endhighlight %}
  * El error que indica Google sobre la línea del email es
<div class="notice--danger">
<p><bold>This page contains the following errors:</bold></p>

<p>error on line 12 at column 85: EntityRef: expecting ';'</p>
<p><bold>Below is a rendering of the page up to the first error.</bold></p>
</div>


