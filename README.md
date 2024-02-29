# Angular 14 + Prettier + Husky Setup

The purpose of this post, is to share in a simple and hopefully useful way how we can implement Prettier and Husky in an Angular 14 application, but before we start you might be wondering what is Prettier, or maybe more specifically **WHAT THAT HELL IS [HUSKY](https://typicode.github.io/husky)**?

## What is [Prettier](https://prettier.io) 😎?

**Prettier** is an popular and opinionated code formatter. It is a tool that helps you enforce consistent code style across your codebase. It is a great tool when you are working with JavaScript, TypeScript, CSS, and other languages, definitely is a great way to keep formatted code consistently across your team.

## What is [Husky](https://typicode.github.io/husky) 🐶?

**Husky**: Of course! Let's focus on this impressive tool rather than discussing the charming breed of dogs known as Huskies.

Husky is described as a tool that simplifies Git hooks. It offers pre-commit and pre-push hooks, which are essentially commands you want to execute each time you commit or push changes.

In this post, we'll demonstrate how to set up Husky and Prettier in an Angular 14 application to streamline and automate code formatting, and detect potential issues/errors in your code.

## Let's get started!

### 1. Create a new Angular 14 project

Create a new Angular 14 project using the CLI and run the project with the following command (in this case the project name is `angular-prettier-husky-setup`, you can name it whatever you want):

_Make sure you have installed NodeJS version 16 minimum, in order to install the Angular CLI, if you don't have it installed on your operating system, for more details on how to install NodeJS, see this [link](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm), as well as the documentation of (Angular CLI)[https://angular.io/cli], for more details on the Angular CLI, and its installation process._

```bash
ng new angular-prettier-husky-setup
```

_Feel free to use the package manager of your choice to install the Angular CLI._

## 2. Install ESlint

Angular nos provee de un poderoso ESlint, que nos ayuda a detectar y corregir los errores de nuestro código. Por lo tanto, vamos a instalarlo en el proyecto.

Para ello podemos guiarnos de la siguiente documentacion, para saber que version de ESlint nos ofrece:

> ng add @angular-eslint/schematics@12 #for angular 12

> ng add @angular-eslint/schematics@13 #for angular 13

> ng add @angular-eslint/schematics@14 #for angular 14

En este caso al trabajar con Angular 14, usamos la version 14. Por lo tanto, vamos a instalarlo:

```bash
ng add @angular-eslint/schematics@14
```

Esto servirá para que automáticamente instale las dependencias necesarias y a su vez realice las configuraciones necesarias como añadir en `angular.json` el registro de los diferentes Schematics usados en la propiedad "schematicCollections" entre otros ajustes.

![alt text](image.png)

Pero que cambios se hicieron, buenos analicemos los cambios que se hicieron en los archivos modificados:

`.eslintrc.json`: En este archivo se encuentran las reglas del linter que usaremos para comprobar si el codigo cumple con las reglas establecidas, y en caso contrario darnos un feedback acerca de los errores que se han producido.

Por defecto, el archivo `.eslintrc.json` se encuentra en el directorio raiz del proyecto y contiene la configuración por defecto.

```json
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "parserOptions": {
        "project": ["tsconfig.json"],
        "createDefaultProgram": true
      },
      "extends": [
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates"
      ],
      "rules": {
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        "@angular-eslint/component-selector": [
          "error",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ]
      }
    },
    {
      "files": ["*.html"],
      "extends": ["plugin:@angular-eslint/template/recommended"],
      "rules": {}
    }
  ]
}
```

`angular.json`: En este archivo se encuentra la configuración de la aplicación Angular, este archivo se modifica en automatico pata añadir el registro de las colecciones de schematics la información del schematics que acabamos de instalar

![alt text](image-1.png)

`package.json`: Se añadio un nuevo script llamado `ng lint` para que se ejecute el linter de Angular, asi como la actualizacion de las dependencias.

![alt text](image-2.png)

Teniendo todos los cambios, podemos continuar con los siguientes pasos.

## 3. Instalar Prettier

Para que se realice la corrección de los errores de nuestro código, vamos a instalar Prettier como una dependencia de desarrollo en nuestro proyecto, para ello vamos a utilizar el siguiente comando:

_(siente libre de usar el gestor de paquetes que prefieras, en este caso y durante todo el resto del post, vamos a utilizar el gestor de paquetes de npm)_

```bash
npm install --save-dev prettier
```

## 4. Configurar Prettier

Vamos a configurar Prettier para que se realice la corrección de los errores de nuestro código, para esto vamos a crear un archivo llamado `.prettierrc` en la raiz del proyecto, y añadiremos la siguiente configuración sugerida:

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "tabs": false,
  "singleQuote": true,
  "semicolon": true,
  "quoteProps": "preserve",
  "bracketSpacing": true
}
```

`printWidth`: Define el ancho máximo de línea antes de que Prettier haga saltos de línea automáticos para mantener el código dentro de este límite.

`tabWidth`: Establece el ancho de tabulación, es decir, cuántos espacios representará un tabulador.

`tabs`: Un booleano que indica si se deben usar tabuladores en lugar de espacios.

`singleQuote`: Un booleano que especifica si se deben usar comillas simples en lugar de comillas dobles para cadenas.

`semicolon`: Un booleano que indica si se deben incluir puntos y comas al final de las declaraciones.

`quoteProps`: Define cómo Prettier manejará las comillas alrededor de los nombres de propiedad de objetos. En este caso, "preserve" significa que mantendrá las comillas si son necesarias para la compatibilidad con versiones anteriores.

`bracketSpacing`: Un booleano que indica si debe haber un espacio después de los paréntesis de apertura y antes de los paréntesis de cierre en los literales de objetos.

Ahora crearemos un archivo en la raiz del proyecto llamado `.prettierignore` y anadiremos la siguiente configuración:

```prettier
package.json
package-lock.json
yarn.lock
node_modules
dist
```

Esto permite que Prettier ignore los archivos que no queramos que se realicen la corrección de los errores de nuestro código, por lo que no deberemos de preocuparnos de que sean analizados por Prettier.

## 5. Instalar Pretty Quick

Que es Pretty Quick?

Pretty Quick es una herramienta que combina dos herramientas populares de JavaScript: Prettier y ESLint, para formatear y revisar tu código de manera eficiente. Prettier se encarga del formateo automático del código, mientras que ESLint se utiliza para realizar verificaciones estáticas del código en busca de errores o prácticas no deseadas.

Pretty Quick simplifica el proceso de formateo y revisión del código al ejecutar ambas herramientas en paralelo. Esto significa que puedes formatear y revisar tu código con una sola línea de comando o integrarlo fácilmente en tu flujo de trabajo de desarrollo.

Ademas solo queremos que prettier solo realice la correccion de errores en nuestros archivos modificados, en lugar de que ESLint realice la corrección de errores en todos los archivos del proyecto, es por ello que tambien Pretty Quick solo analizará los archivos modificados en nuestro proyecto, y para ello debemos ejecutar el siguiemnte comando para instalar Pretty Quick como una dependencia de desarrrollo en nuestro proyecto:

```bash
npm install --save-dev pretty-quick
```

Una vez instalado Pretty Quick, añadiremos un nuevo script en el archivo `package.json`, que se encargará de realizar la corrección de errores solo en los archivos que han sido preparados para ser confirmados en Git, con la ayuda de la bandera `--staged`.

```json
{
  "scripts": {
    "pretty-quick": "pretty-quick --staged"
  }
}
```

# Instalar Husky

Bien, ahora podemos realizar la instalacion de Husky, por fin.
Para ello debemos seguir la documentacion y ejecuar el siguiente comando:

```bash
 npx husky-init && npm install
```

Esto creara un nuevo directorio, en la raiz del proyecto, llamado `.husky`, y todos los archivos necesarios para que el comando `husky` funcione.

Por defecto, dentro de estar carpeta encontraras un archivo llamado `pre-commit` con el siguiente contenido:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm test
```

en donde reemplazamos `npm test` por el comando que queramos ejecutar, en este caso `npm run pretty-quick`.

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run pretty-quick
```

Recuerda que este comando se añadio en el archivo `package.json`, en el paso #2.

Ahora añadiremos un nuevo archivo dentro de la misma carpeta `.husky` llamado `commit-msg` con el siguiente contenido:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run lint
```

_Nota: si en tu proyecto estas realizando pruebas unitarias, podrias añadir un nuevo archivo en la misma carpeta `.husky` llamado `pre-push` con el siguiente contenido:_

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run test
```

_para ejecutar las pruebas unitarias antes de realizar el push, a la rama remota de tu repositorio en la que estas trabajando_

Para este ejemplo sencillo, solo utilizaremos los archivos `.husky/pre-commit` y `.husky/commit-msg`.

# Probar la corrección de errores Prettier + ESLint + Husky

Ahora que ya tenemos el proyecto listo para ejecutar los comandos de pre-commit y commit-msg, vamos a probar que todo funciona correctamente.

Para ello vamos a crear un error en nuestra aplicacion, para ello nos iremos al archivo `app.component.html` y lo modificaremos de la siguiente manera:

```html
<h1>Angular 14 + Prettier + Husky</h1>

<div>
  <span> Este es un texto que NO debe Ser Valido Para Nuestro Linter Prettier </span>
</div>
```

y en el archivo `app.component.ts`: implementaremos el cliclo de vida `OnInit`, pero dejaremos el contenido del ciclo de vida vacio para que el linter nos lo indique como un error.

![alt text](image-3.png)

Bien, ahora es momento de probar que todo funcione correctamente, para lo cual con husky configurado, y utilizando los hook `pre-commit` y `commit-msg` vamos a ejeuctar los siguientes comandos:

1. Añadiremos los archivos al staged de Git:

```bash
git add .
```

Esto nos permitirá confirmar que todos los archivos han sido preparados para ser confirmados por husky.
