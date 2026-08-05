---
título: Sistema de wiki en Markdown fusionado de Wikitten y MDwiki
autor: Uranus Zhou
---

# Sistema de wiki en Markdown fusionado de Wikitten y MDwiki

## Introducción

Este sistema de wiki fusionado de Wikitten y MDwiki se basa en el wiki PHP [Wikitten](http://wikitten.vizuina.com/) y en el wiki puramente estático en JavaScript [MDwiki](http://www.mdwiki.info/).

Puedes implementar este sistema de wiki fusionado en tu servidor web PHP (a través de Wikitten) o en un servidor web estático, y también puedes visualizar tu wiki localmente (a través de MDwiki).

Realicé algunas modificaciones en MDwiki y Wikitten para que ambos puedan compartir el mismo directorio de documentos wiki y mostrar una interfaz de wiki similar.

### Cambios en MDwiki

* Todos los documentos Markdown se almacenan en un nuevo directorio llamado `library`, que es el mismo que el utilizado por Wikitten.
* Se ha añadido soporte para [YAML front matter](http://jekyllrb.com/docs/frontmatter/) al inicio de los archivos Markdown.
 
 > **Nota**
 > 
 > El soporte para YAML front matter ya está disponible en la versión de desarrollo de MDwiki y estará disponible para los usuarios finales en una próxima versión.
 > El YAML front matter en un documento Markdown debe comenzar y terminar con tres comillas invertidas (`` ` ``) en lugar de tres guiones (-).
 
* Desarrollé un script de generación automática de índices llamado **`generate-index.sh`** para el directorio `library` de MDwiki. Ejecútalo en una terminal Bash:
 
 ```bash
 ./generate-index.sh library
 ```
 
 El script **`generate-index.sh`** generará un archivo `index.md` para todos los subdirectorios (solo si un directorio no contiene un archivo `index.md` válido).

 Después de agregar un nuevo documento Markdown o eliminar/renombrar un documento en el directorio `library`, debes ejecutar el script **`generate-index.sh`** para generar nuevos archivos de índice.

* Se ha añadido soporte para búsqueda completa de contenido en documentos Markdown mediante una versión modificada de **lunr.js**. Desarrollé un programa Node.js de generación automática de índices de búsqueda llamado **`search.js`**. Tanto MDwiki como Wikitten utilizan el archivo de índice **search_index.json** en el directorio del programa wiki.

 ```bash
 node search.js library search_index.json
 ```
 
 Si necesitas soporte para búsqueda completa de contenido, debes ejecutar el script **`search.js`** después de realizar cualquier cambio en la biblioteca wiki.

### Cambios en Wikitten

* Se ha añadido soporte para YAML front matter que comience y termine con tres comillas invertidas (`` ` ``) y tres guiones (-).
* Se muestra el documento `index.md` al cambiar a un directorio (en lugar de pedir al usuario que seleccione uno en el árbol de la biblioteca wiki).
* El ancho de las tablas Markdown se establece en el 100 %.
* Se ha añadido soporte para búsqueda completa de contenido en documentos Markdown. Utiliza el programa Node.js **`search.js`** para generar el archivo de índice de búsqueda **search_index.json**.

## Implementación

Nota: Wikitten y MDwiki comparten el mismo directorio `library` para almacenar documentos Markdown.

### Implementación de Wikitten

Consulta el sitio web de [Wikitten](http://wikitten.vizuina.com/) para obtener más información. Necesitas al menos PHP 5.3 y el módulo php_fileinfo, y tu servidor web debe soportar reescritura de URLs.

El archivo de configuración de Wikitten es `config.php` y se encuentra en el directorio raíz.

 > **Nota**
 > 
 > Si necesitas soporte para búsqueda completa de contenido, debes modificar la regla de Apache o Nginx para **search_index.json**, ya que de lo contrario será servido por defecto por el PHP de Wikitten.
 > 
 > Un ejemplo de regla de Nginx podría ser:
 > 
 > ```
 > location ~* ^/search_index.json$ {
 >     access_log off;
 >     expires max;
 > }
 > ```

### Implementación de MDwiki

Consulta el sitio web de [MDwiki](http://www.mdwiki.info/) para obtener más información.

El archivo de configuración de MDwiki es `config.json` y se encuentra en el directorio raíz. He añadido una nueva configuración llamada `parseHeader` en el archivo `config.json` (esta función ya está disponible en la versión de desarrollo de MDwiki).

## Contacto

Si tienes algún problema, no dudes en abrir un nuevo issue o contactarme en [https://zohead.com/](https://zohead.com/).
