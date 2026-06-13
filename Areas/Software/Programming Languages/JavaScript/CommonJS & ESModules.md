---
tags:
  - areas
  - software
  - programming-languages
  - javascript
author: Santi Tabbach
created: 2026-05-24
modified: 2026-06-13
---
## CommonJS

Nace para poder partir el código en diferentes archivos y poder comunicarlos entre si mediante exportación/importación de la siguiente manera:

```javascript
// math.js - EXPORTAR
const sum = (a, b) => a + b;
module.exports = { sum };

// app.js - IMPORTAR
const { sum } = require('./math');
console.log(sum(2,3)); // 5
```

Características clave de CommonJS:

- `require()` **es una función** - se ejecuta en runtime, como cualquier otra función.
- **Es síncrono** - cuando haces `require('./math')`, Node FRENA la ejecución, lee el archivo, lo ejecuta, y te devuelve lo que exportó.
- **Es dinámico** - podés hacer cosas como `require(variable)` o meter un `require` dentro de un `if`

```javascript
// Esto es válido en CommonJS
if(process.env.NODE_ENV === 'production'){
	const logger = require('./prod-logger');
} else {
	const logger = require('./dev-logger');
}
```

Esto funciona bien en un servidor porque leer un archivo del disco es rápido. Pero en un navegador, donde cargar un archivo significa hacer un request HTTP... **síncrono es inaceptable**. Frenaría toda la página.

---

## ESModules: el estándar oficial del lenguaje (ES2015/ES6)

En 2015, el comité TC39 (los que diseñan JavaScript) finalmente agregó un sistema de módulos al lenguaje. No es una librería, no es un invento de Node - es parte de la especificación de javascript:

```javascript
// math.js - EXPORTAR
export const sum = (a, b) => a + b;

// app.js - IMPORTAR
import { sum } from './math.js';

console.log(sum(2, 3)); // 5
```

Diferencias fundamentales con CommonJS:

#### 1. Es estático (se analiza ANTES de ejecutar)

```javascript
// ESTO ES INVÁLIDO en ESModules
if(condition) {
	import { sum } from './math.js'; // Error
}
```

Los `import` DEBEN estar en el nivel superior del archivo, porque el motor de JavaScript necesita saber QUÉ módulos necesita ANTES de ejecutar una sola línea de código. Esto permite:

- **Tree-shaking**: el bundler puede eliminar código que nunca se usa.
- **Análisis estático**: las herramientas (Typescript, ESLint) pueden verificar imports sin ejecutar el código.
- **Carga paralela**: el navegador puede pedir todos los módulos a la vez, no uno por uno.

#### 2. Es asíncrono por diseño

Fue diseñado pensando en el navegador. Los módulos se cargar sin bloquear.

#### 3. Tiene export default

```javascript
// CommonJS
module.exports = function(){ ... }

// ESModules
export default function(){ ... }
```

---

### La analogía de la construcción

Pensalo como dos formas de construir un edificio:

**CommonJS** es como construir con obreros que van al depósito de materiales SOLO CUANDO los necesitan. Funciona si el depósito está al lado (disco local), pero es un desastre si está a 20km (red).

**ESMódules** es como tener el plano completo de materiales ANTES de empezar la obra. Sabes exactamente que vas a necesitar, podes pedir todo en paralelo, y podes detectar que pediste 1000 ladrillos pero solo usas 200 (tree-shaking).

---

### ¿Cómo se distinguen en la práctica?

![[Pasted image 20260524150839.png]]

---

### Relación con Typescript

TypeScript usa la sintaxis de **ESModules** (`import/export`) y luego, cuando compila, puede transformar eso al formato que vos le pidas en `tsconfig.json`:

```
"module": "ES2020", // Compila a ESModules
"module": "CommonJS" // Compila a require/module.exports
```

La opción `module` en el `tsconfig` le dice a Typescript: "cuando analices el código, esperá que los módulos usen ESTE sistema."