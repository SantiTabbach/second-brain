---
tags: [areas, software, frontend]
author: Santi Tabbach
created: 2026-05-22
modified: 2026-05-23
---

## Indice

1. [[#DOM (Document Object Model)]]
2. [[#Virtual DOM]]
3. [[#Performance]]
4. [[#Técnicas comunes de optimización]]
5. [[#Estado (State)]]
6. [[#Renderizado y Rendering Pipeline]]
7. [[#Métodos de renderizado]]
8. [[#Streaming y Server Components]]
9. [[#Caché]]

---
## DOM (Document Object Model)

El DOM es una representación en memoria del documento HTML que el navegador construye cuando carga una página.
Esa representación tiene forma de árbol: cada etiqueta HTML se convierte en un nodo con relaciones padre/hijo.

Por ejemplo:

```html
<body>
	<button>Comprar</button>
</body>
```

Se representa internamente como algo similar a:

```
body  
└── button
```

Cuando abrís un modal, ocultas un botón, cambias texto, agregas clases CSS o renderizas componentes dinámicos, en realidad estás modificando ese árbol en memoria.

El navegador luego debe:
1. Recalcular estilos (CSS)
2. Recalcular layout/posiciones
3. Redibujar partes de la pantalla (paint)
4. Componer la imagen final (compositing)
Por eso manipular excesivamente el DOM puede impactar directamente en la performance.

---

## Virtual DOM

El Virtual DOM es una representación liviana del DOM real mantenida en memoria por frameworks como React.

La idea principal:

En lugar de modificar el DOM real constantemente:
1. se genera un árbol virtual nuevo,
2. se compara con el anterior,
3. se detectan diferencias,
4. se actualiza sólo lo necesario en el DOM real.

Ese proceso se llama **Reconciliación**.

### Reconciliación

La reconciliación intenta minimizar operaciones costosas sobre el DOM real ya que modificar este último suele ser mucho más caro que operar sobre estructuras JS en memoria.

Importante:

El Virtual DOM NO significa "no renderizar".

Los componentes igual se ejecutan nuevamente.

La optimización está en:
- minimizar escrituras reales al DOM,
- agrupar actualizaciones,
- reducir trabajo del navegador.


---
## Performance 

La performance en frontend consiste en minimizar el tiempo y recursos necesarios para que una aplicación:
- cargue,
- responda,
- renderice,
- y se mantenga fluída.
No se trata solamente de "que cargue rápido", sino también de:
- percepción de velocidad,
- fluidez viusal,
- capacidad de interacción,
- estabilidad visual,
- y consumo eficiente de CPU/memoria.

---
### Métricas objetivas de performance

#### Time to Load

Tiempo que tarda el usuario en comenzar a ver contenido útil desde que ingresa a la página.
Hoy suele dividirse en métricas más especificas como:
- First Contentful Paint (FCP)
- Largest Contentful Paint (LCP)

#### Time to Interactive 

Tiempo que tarda la aplicación en estar realmente lista para interacción.
No alcanza con "ver algo"; la página debe responder correctamente a:
- clicks,
- inputs,
- scroll,
- navegación,
- animaciones.
Una app puede verse cargada pero seguir bloqueada ejecutando JavaScript pesado.

---

### FPS (Frames Per Second)

Cantidad de frames renderizados por segundo.
El estándar de fluidez suele ser de $60s$. Si el render cae mucho debajo de eso aparecen:
- lag,
- stuttering,
- animaciones trabadas,
- scroll poco fluido.

---

## Técnicas comunes de optimización

### Lazy Loading

Consiste en cargar recursos únicamente cuando realmente son necesarios.

Ejemplos
- imágenes al hacer scroll,
- módulos JS bajo demanda,
- modales que se cargan al abrirse,
- rutas/code splitting.

Objetivos:
- reducir peso inicial,
- acelerar el primer render,
- disminuir consumo innecesario.

Ejemplo típico:

```typescript
const SettingsModal = React.lazy(() => import('./SettingsModal'));
```

---

### Minimizar el DOM

Cuanto más grande y complejo es el DOM:
- más costoso es calcular estilos,
- más costoso es recalcular layouts,
- más caro es el render.

Problemas típicos:
- árboles HTML enormes,
- nesting excesivo,
- renders innecesarios,
- miles de nodos ocultos.

Buenas prácticas:
- renderizar solo lo visible,
- usar virtualización en listas grandes,
- evitar wrappers innecesarios,
- desmontar componentes ocultos cuando no se usan.

---

### Evitar renders innecesarios

En frameworks modernos, muchas optimizaciones giran alrededor de reducir renders redundantes.

Porque renderizar implica:
- ejecutar componentes,
- actualizar DOM,
- recalcular estilos.

Herramientas comunes:
- memoización (`React.memo` , `useMemo`, `useCallback`)
- selectors eficientes
- normalized state

---

### Virtualización

Técnica para renderizar únicamente los elementos visibles en pantalla.

Ejemplo:

Una tabla con 50.000 filas NO renderiza 50.000 nodos HTML simultáneamente. Renderiza quizás 20 filas y el resto quedan en el buffer.

Bibliotecas comunes:
- react-window
- react-virtualized
- FlashList

---

## Estado (State)

El estado representa la "fotografía actual" de la aplicación en un momento dado.

Ejemplos:
- modal abierto/cerrado,
- checkbox marcado,
- texto escrito en un input,
- filtros activos,
- datos obtenidos del backend.

La UI es una representación visual del estado actual.

---
### Tipos de estado

#### Estado Local

Existe únicamente en el cliente (frontend) y normalmente afecta una parte específica de la UI.

Ejemplos:
- errores de validación,
- dropdown abierto,
- tabs activas,
- loading de un botón.

El backend no necesita conocer estos estados.

#### Estado del Backend / Server State

Proviene del servidor y representa información persistida o compartida.

Ejemplos:
- usuarios,
- productos,
- pedidos,
- mensajes,
- permisos.

Características:
- asincrónico,
- cacheable,
- sincronizable,
- potencialmente stale.

#### Estado combinado

Ocurre cuando el frontend mezcla:
- estado local,
- estado remoto,
- reglas de negocio.

Ejemplo:
1. El usuario escribe un email válido sintácticamente.
2. El backend responde: *"el email no existe"*.
3. El frontend actualiza su estado local.
4. La UI muestra el error.

---

### Consistencia e idempotencia visual

La UI debe mantenerse coherente con el estado más reciente.

Ejemplo incorrecto:
1. Buscar “remeras” → aparecen resultados.
2. Buscar “zapatos”.
3. El backend devuelve vacío.
4. La UI sigue mostrando remeras anteriores.

Eso genera inconsistencia visual.

La UI debe:
- limpiar estados obsoletos,
- invalidar resultados anteriores,
- evitar race conditions,
- manejar correctamente loading/error/empty states.

---
## Renderizado y Rendering Pipeline

Renderizar no significa solamente "mostrar HTML".

El navegador atraviesa múltiples etapas:

```
HTML → DOM  
CSS → CSSOM  
DOM + CSSOM → Render Tree  
Render Tree → Layout  
Layout → Paint  
Paint → Composite
```

Cada modificación puede invalidar parte de este pipeline.

Algunas operaciones son especialmente caras:
- cambiar tamaños,
- mover elementos,
- recalcular layout complejos.

Por eso animar con:

```css
transform
opacity
```

suele ser más eficiente que modificar:

```css
top
left
width
height
```

---

## Métodos de renderizado

### CSR (Client Side Rendering)

El servidor envía un HTML mínimo y JavaScript.

El cliente ejecuta el JS y construye/renderiza la aplicación.

Flujo:

```
cliente → pide página
server → devuelve HTML + JS
cliente → ejecuta JS
cliente → renderiza UI
```

Ventajas:
- aplicaciones altamente interactivas,
- navegación fluida,
- menos trabajo del servidor.

Desventajas:
- peor SEO (históricamente),
- peor performance en dispositivos lentos,
- bundle JS grandes afectan TTI.

---

### SSR (Server Side Rendering)

El servidor renderiza el HTML antes de enviarlo.

Flujo:

```
cliente → pide página
server → ejecuta render
server → devuelve HTML listo
cliente → hidrata la app
```

Ventajas:
- mejor SEO,
- contenido visible rápidamente,
- mejor percepción inicial.

Desventajas:
- más carga en el servidor,
- mayor complejidad,
- hydration cost en cliente.

Importante:
Aunque el HTML llegue renderizado, normalmente el cliente igual necesita ejecutar JS para "hidratar" la app y volverla interactiva.

#### Hydration
Consiste en conectar el HTML renderizado con la lógica JavaScript del frontend.

---

### SSG (Static Site Generation)

El HTML se genera durante la build.

No se renderiza ni en cliente ni por request en servidor.

Ideal para:
- blogs,
- documentación,
- landing pages,
- marketing sites,
- portfolios.

Ventajas:
- extremadamente rápido,
- cacheable vía CDN,
- muy barato de servir.

Desventajas:
- contenido menos dinámico,
- rebuilds necesarios para cambios.

---

## Streaming y Server Components

Tendencias modernas en frameworks como Next.js:

### Streaming SSR

El servidor envía HTML parcialmente mientras sigue renderizando el resto, haciendo que la percepción de velocidad sea más alta.

### React Server Components

Permiten ejecutar ciertos componentes exclusivamente en servidor:
- reduciendo JS enviado al cliente,
- disminuyendo hydration,
- mejorando performance inicial.

---

## Caché

La caché es fundamental en performance frontend.

Puede existir en:
- navegador,
- CDN,
- service workers,
- memoria local,
- server,
- queries de datos.

Objetivos:
- evitar requests innecesarios,
- reducir latencia,
- disminuir carga de servidor.