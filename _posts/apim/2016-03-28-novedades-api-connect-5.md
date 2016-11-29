---
title: "Novedades API Connect 5"
categories:
  - APIm
tags:
  - APIm
  - IBM
  - APIConnect
excerpt: "Lista de novedades de IBM API Management V5 (ahora conocido como API Connect)"
header:
  teaser: /assets/images/apiconnect_logo.png
---

Lista de [novedades](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_whatsnew.html?lang=en) de IBM API Management V5 (ahora conocido como API Connect) 


Como indica el que se haya cambiado hasta el nombre del producto, hay bastante cambios con respecto a la versión anterior.

En la parte positiva:
- Los planes se recubren con un nuevo elemento: Producto.
  - Creo que esta parte me va a gustar, ya que los planes por si solos no servían para crear "productos" combinando APIs y consumos
- Hay mucha más APIs internas documentadas
  - Por ejemplo, para el [portal](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.apirest.doc/rest_apis.html?lang=en) 
- Parece que mucha más información de un API se exporta de una forma estandarizada, usando extensiones Swagger 
- Se ha eliminado el portal básico.
  - No aportaba nada y podía llevar a confusión si alguien entraba en uno.
- Existe una edición [libre (Essential)](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.overview.doc/overview_rapic_offerings.html?lang=en) con la que podemos probar y jugar con la solución en desarrollo 
  - Aunque no creo que se pueda descargar alegremente y sin licencia :disappointed:
- Se amplia el producto y además de cubrir la parte de gestión y seguridad (manager y gateway/datapower) , se incluyen facilidades para diseñar y ejecutar las APIs
  - O sea, IBM amortizando la compra de LoopBack
- Nuevas políticas de caja, aunque llegan tarde y muchos se las habrán tenido que hacer a medida en V4
- Nuevas facilidades para personalizar los formularios OAuth

Cosas que no me gustan:
- No veo que se pueden aplicar políticas a nivel de producto o plan
- Los entornos pasan a llamarse catálogos, pero siguen asociados a un site de portal específico.
  - Quiero que desde el portal pueda probar contra un API real de producción o contra un mockup de sandbox.
- Me parece muy bien tener un toolkit , con el que poder crear APIs desde la línea de comandos. Pero, 
  - ¿por qué no un API REST documentado para integrarme como yo quiera? Y no me vale con mirar las fuentes del toolkit en node a ver a donde llaman.
- Está muy bien tener un micro-gateway para entornos no productivos y para la licencia "Essential" gratuita.
  - Pero veo riesgos en esta aproximación ya que el micro-gateway (basado en Node) no tiene las mismas política que Datapower.
    - Esto nos lleva a que tendremos APIs que funcionan en un entorno local, al ejecutarse en el micro gateway en node, y que luego no funcionen o tengan otro comportamiento en el gateway real en producción con Datapower
    - Es más, al definir el API, hay elementos que [cambiarán](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.policy.doc/capim_custpolicies_overview.html?lang=en) entre ejecutar en el micro gateway o ejecutar en Datapower. 
    - Como muestra, la política de gatewayscript está solo disponible para [Datapower](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/rapim_cli_policies_gwscript.html?lang=en)
    - ... y la política de [javascript](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.toolkit.doc/rapim_cli_policies_jsscript.html?lang=en) solo está disponible para el micro gateway
- Se mejoran las analíticas, pero sigo sin ver algo para exportar (en tiempo real) el detalle de las invocaciones.
  - Muy triste que se siga ofreciendo solo exportación a CSV (y encima sin API REST para automatizarla) , impidiendo aprovechar fuera del producto el nuevo y completo [CDR](http://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.apionprem.doc/rapim_analytics_apieventrecordfields.html?lang=en)

En resumen, buenos cambios, pero al producto le siguen faltando facilidades de integración (API REST para toda la gestión de APIs, convivencia con servidores OAuth de terceros, exportación analíticas, etc.)