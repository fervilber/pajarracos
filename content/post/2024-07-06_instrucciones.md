+++
title = 'Instrucciones'
date = 2024-05-14T07:07:07+01:00
draft = false
+++

# Instrucciones para empezar con el blog
Este es un post especial, porque quiero probar a ver qué diablos pasa cuando renombro y canto con las sirenas en la playa de san peter.
![](https://static.wikia.nocookie.net/mitologiagriega/images/5/57/Grifo.png/revision/latest?cb=20140630222640&path-prefix=es)

## Nada
y la *realidad* es que nada de esto sería real, si no hubiera grifos volando ahora ismo por mi ventana.

## cómo hacer el blog con HUGO en github pages
Bueno, la cosa siempre es más dificil de lo que parece, pero voy a intentar poner todo el proceso claro.
en primer lugar me he instalado HUGO en mi PC usando **winget** de windows, que básicamente ha sido abrir un terminal en bash y poner esto:

```
winget install Hugo.Hugo.Extended
```
Así se ha instalado y ocupa poco unos 30 mg, mucho menos que jekyll.
Después he abierto un terminal con git bash que ya lo tenía intalado y me voy a una carpeta donde quiero hacer el primer blog.
Sigo las instrucciones de esta web <https://gohugo.io/getting-started/quick-start/>

Y es que una vez en la carpeta elegida pongo esto, ya que la carpeta que quiero crear se llama blog.
```
hugo new site blog
```
Luego me voy a la carpeta con esto:

`cd quickstart`

## inicializar GIT
Ahora toca iniciar git en local para ello hacemos lo siguiente desde el terminal:

`git init`

En HUGO hay que seleccionar una plantilla si queremos cambiar la que viene por defecto, para ello podemos hacer esto que en mi caso es para usar la plantilla Mainroad de vinux

`git submodule add https://github.com/Vimux/Mainroad themes/Mainroad`

Con esto copiamos la plantilla en local a la carpeta de themes local y ya la podemos usar.
hay que añadir en el fichero `hugo.toml` una linea qie ponga `theme = Mainroad`.

Para probar la web en local se puede poner en el terminal esto y se ejecuta el servidor:

`hugo server`

en el terminal dice la dirección para ver la web que será algo así como:

`localhost:1313`

### Para crear un nuevo post con código 
basta con hacer esto:

`hugo new content content/post/mi-primer-post.md`

con esto nos crea un fichero en el directorio post que es donde debemos ponerlos con el encabezado simple .
Aunque posteriormente podemos copiar y pegar un plantilla que tengamos por ahí. Una cosa importante es eque mientras la linea de _draft=true_ este así el post no se publica, para que se publique hay que cambiarlo a false.

Para crear las web y verla se puede hacer con este comando:

`hugo server -D`

aunque si queremos ver las webs de borrador, las de draft=true pondemos hacer:

`hugo server --buildDrafts`
`hugo server -D`

### Configuracion de theme
Para configurar todas las multiples opciones que tiene la plantilla hay que cambiar el fichero hugo.toml
y una cosa importante es poner la baseurl
```
baseURL = 'https://fervilber.github.io/pajarracos/'
languageCode = 'es'
title = 'MY flamante blog'
theme = 'Mainroad'

```

## Publicar el sitio en github

Para publicar hay que hacer primero hacer el `hugo` para imprimir todas las paginas estáticas esto se llama deploy.
En github se debe hacer con actions

así que antes hay que crear el sitio en github de cero y enlazar el origen de a esto,.

Una vez creado el repo en github nos vamos al local y enlazamos con esto:

```
git status
git add . 
git commit -m "Nuevo blog"
git remote add origin https://github.com/fervilber/pajarracos.git
git push -u origin master
```

Cuando hacemos cambios en local, siempre antes haremos un git pull para enlazar con github, despues hacemos los cambios y tras eso hay que hacer otra vez un git add ., y commit u por ultimo el push

## actions en github
para que github publique esta web hay que hacer un deploy y antes ir a configuración: pages y cambiar  deploy from the branch a **GitHub Actions**
![](https://images.prismic.io/staticmania/60bcb6cc-bed4-4cce-bbae-ef42590c7ad4_Screenshot+2023-09-04+160103.png?auto=compress,format)

esto lo explica bien aquí: <https://staticmania.com/blog/deploy-your-hugo-website-on-github-pages>

Al hacer esto el mismo github te recomienda unas actions justo debajo, si no está Hugo, hay que buscarla en la opcion que dan. y al final esto crea una nueva carpeta en el directorio de trabajo llamada  `.github/workflows` donde crea un fichero llamado `hugo.yml` . el que me ha creado a mi es este y funciona:

```
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.128.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```

cada vez que hagamos un commit se ejecutará automáticamente esta action que podemos ver en la pestalña correspondiente y si falla podemos leer por qué. A mi me ha fallado varias veces hasta que he actualizado el theme de HUGO a la ultima versión.




