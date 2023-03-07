---
title: Despliegue en Hostinger
date: 2023-03-07 12:39:16
tags:
---

Este blog está alojado en Hostinger, que se actualiza automáticamente cada vez que hago un cambio en un repositorio alojado en GitHub.
Para conseguir un comportamiento similar, sube a tu repositorio un archivo en `.github/workflows/deployment.yaml` con el siguiente contenido:

``` yaml
name: Build static files

on:
  push:
    branches:
      - main

jobs:
  pages:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          submodules: recursive

      - name: Use Node.js 18.x
        uses: actions/setup-node@v2
        with:
          node-version: '18'

      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache

      - name: Hexo
        run: |
          npm i -g hexo-cli
          npm i
          hexo clean && hexo g

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

Entre otros, lo que se hace es utilizar node en su versión 18 para instalar las dependencias, correr Hexo (el framework utilizado) para generar los archivos estáticos, y gracias a reutilizar una action dedicada a publicar en Github Pages, se crea una nueva rama `gh-pages` con el contenido de la carpeta `./public`.

En Hostinger, lo primero es limpiar la carpeta `public_html` de archivos, ya que puede incurrir en problemas. Una vez hecho, se puede ir a `Avanzado -> Git` y aquí se tienen dos opciones:

__Repositorio privado de Github__

Si se utiliza un repositorio privado, crear una clave SSH, copiar el contenido y posteriormente ir a Github `Settings -> SSH and GPG keys` y añadir una nueva clave SSH con el contenido copiado anteriormente.
Como URL del repositorio tendremos que utilizar la que empieza por `git@github.com:...`.

__Repositorio público de Github__

En este caso es mucho más sencillo, ya que no hace falta configuración previa, simplemente utilizar la URL que empieza por `https://github.com/...`

---

Una vez se tiene la URL, se escriben en la misma web de Hostinger, en el apartado `Repositorio`, en el branch se escribe `gh-pages` que es donde estarán nuestros archivos estáticos y en Directorio se deja en blanco (es la carpeta dentro de `public_html` donde se copiará en contenido). Una vez esté todo configurado, creamos el repositorio y entre los botones que aparecen más abajo, se selecciona el de `Implementación automática` y obtendremos algo parecido a esto:

{% asset_img webhook.png %}

Si entramos en el enlace de configuración de webhook de Github, llegaremos a un formulario en el que nos permitirá crear uno nuevo. Ahí solamente debemos insertar la URL del webhook (con cuidado de no perder ningún carácter del final), y en el tipo de payload, seleccionar JSON.

Una vez esté configurado, no tendremos más que hacer push con cambios o nuevos posts al repositorio, Github se encargará de generar los nuevos archivos y avisará a Hostinger, que los recogerá y pondrá a disposición de acceso web.