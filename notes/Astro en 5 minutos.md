---
id: 1cd6dd
title: "Astro en 5 minutos"
imgs: []
created: 2025-05-19T12:28:00.000-04:00
modified: 2025-05-21T18:16:00.000-04:00
tags: ["type/sprout", "queued/note"]
alias: ["Astro En 5 Minutos", "astro en 5 minutos"]
---

🔗: [^ref1] [^ref2] [^ref3] [^ref4] [^ref5] [^ref6]

> [!NOTE] ¿Que es Astro?
>
> Es un **framework** para desarrollar páginas web **estáticas orientadas a contenido**
>
> - Permite el uso y combinación diversos frameworks de frontend
>   - React, Vue, Preact, Svelte y Solid
> - A nivel de sintaxis, utiliza algo parecido al JSX de React.

## Características de Astro

> [!NOTE]+ Astro tiene todo lo necesario para el desarrollo web moderno
>
> - Colecciones de contenido.
> - Por defecto, retorna 0 javascript al cliente.
> - Transiciones entre vistas (páginas).
> - Middleware.
> - Manejo de acciones del cliente desde el servidor (actions).
> - Una API para setear y gestionar variables de entorno.
> - Adaptadores de despligue para la integración en diversos hostings.
> - Integración con diversos frameworks de UI.
> - Dev Toolbar.
> - Plantillas de proyecto predefinidas.

## Iniciar un proyecto

> [!IMPORTANT] Para iniciar un proyecto de Astro...
>
> ```npm
> # La sintaxis es muy similar a otros gestores de paquetes
> npm create astro@latest
> ```
>
> - Esto inicia el asistente de configuración inicial de proyectos de Astro.
> - En el `package.json` veremos las dependencias instaladas y comandos.

### Instalar paquetes

> [!IMPORTANT] Si quisieramos instalar paquete adicionales:
>
> ```npm
> npm astro add <package> # p.ej. tailwind 
> ```

### Estructura inicial

> [!NOTE] La estructura desde la que parte un nuevo proyecto de Astro es:
>
> ```text
> (root)
>   |-> public/ # Lugar de los archivos estáticos.
>   |-> src/
>        |-> components/
>        |-> layouts/ # Plantillas sobre las que se construyen las páginas.
>        |-> pages/
> ```

#### Páginas

> [!NOTE] Una página es cualquier archivo dentro de `src/pages`.
>
> - Los tipos soportados son: `.astro`, `.md`, `.mdx` o `.html`.
>   - Las páginas `.astro` funcionan igual que un componente de Astro.
>   - En el caso de `.md` y `.mdx`, pueden cargar plantillas con la propiedad `layout`.
>   - Los `.html` no son compatibles con todas las características de Astro.

##### Estructura de una página de Astro

> [!NOTE] Las páginas (y componentes) `.astro` tienen la siguiente estructura:
>
> ```astro
> ---
> // Scripts de la página (Javascript)
> ---
> <!-- Maquetación de la página (HTML + Expresiones JS) -->
> ```

#### Plantillas

> [!NOTE] Las plantillas son componentes que definen la estructura de una página.
>
> Son archivos `.astro` ubicados dentro de `src/layouts`.
>
> - Ofrecen elementos de UI y scripts comunes para las páginas que las utilizan.
> - Tienen las mismas funcionalidades que cualquier otro componente de Astro.

##### Estructura de una plantilla

> [!NOTE] Las plantillas de Astro se estructuran de la siguiente manera:
>
> ```astro
> ---
> // Scripts de la plantilla (Javascript)
> ---
> <html>
>   <head> ... </head>
>   <body>
>     ...
>     <slot /> <!-- Aquí es donde se inyectan las páginas -->
>   </body>
> </html>
> ```

### Colecciones

> [!NOTE] Una coleccion es una agrupación de entradas de contenido.
>
> - Se definen como directorios de alto nivel dentro de `src/content`.
>   - Pueden contener archivos `.md`, `.mdx`, `.markdoc`, `.yaml` o `.json`
> - Las colecciones también se pueden obtener de fuentes externas, como APIs.

#### Configurar una colección

> [!IMPORTANT] Para poder utilizar una colección, primero tenemos que definirla.
>
> Creamos dentro del directorio de la colección un fichero `config.ts`:
>
> ```typescript
> import { defineCollection, z } from "astro:content";
> // La z es de Zod, una librería de validación de datos.
> 
> const <collectionName> = defineCollection({
>   schema: z.object({
>     <frontmatterProp>: <zodType>,
>   })
> })
> 
> export const collections = { <collectionName> }
>   ```

#### Obtener entradas de contenido

> [!IMPORTANT] Para obtener las entradas de una colección, utilizamos el método:
>
> `await getCollection(<collectionName>)`

## Enrutamiento

> [!NOTE] El routing de Astro se basa en la estructura de ficheros dentro de `src/pages`.
>
> En función del nombre del archivo, tenemos 2 tipos de rutas:
>
> - Estáticas: Apuntan a archivos con nombre completo -> `<pageName>.astro`.
> - Dinámicas: Apuntan a archivos con parámetros dinámicos -> `[<param>].astro`.
>   - REST: Permiten encadenar varios parámetros en una sola ruta -> `[...<param>].astro`.

### Enrutamiento dinámico: SSG y SSR

> [!NOTE] El enrutamiento dínamico utiliza SSG o SSR para generar las páginas.
>
> - **SSG (Static Site Generation)**: Genera las páginas **en el momento de la compilación**.
>   - Es la generación por defecto.
>   - Necesita la definición de las rutas disponibles y el contenido a mostrar.
>     - Esto se hace mediante el método `getStaticPaths`.
> - **SSR (Server Side Rendering)**: Genera las páginas **en el momento de la petición**.
>   - Responden a cualquier llamada que coincida con el patrón de la ruta.

#### Colecciones y enrutamiento dinámico

> [!NOTE] El enrutamiendo dinámico hace uso de colecciones para generar páginas.
>
> - Con SSG, utilizamos `getStaticPaths` para definir las rutas y el contenido:
>
> ```astro
> export async function getStaticPaths() {
>   return [
>     {
>       // Parámetros correspondientes a los indicados en el nombre del fichero.
>       params: {
>         // Multiples parámetros
>         { param_1: "param_1", param_2: "param_2" }, // [param_1]-[param_2].astro
>         // Ejemplo REST
>         { path: "<slug_1>/<slug_2>/<slug_3>" }, // [...slug].astro
>       },
>       // Datos que se pasan a la página y que no se incluyen en la URL.
>       props: { ... },
>     },
>   ]
> }
>   
> //--- Usando getCollection() ---//
>   
> export async function getStaticPaths() {
>   pages = await getCollection("\<collectionName\>");
>   
>   // Obtenemos el slug y los datos de la colección.
>   return pages.map((page) => ({
>     params: { slug: page.slug },
>     props: { page }
>   })
> }
> 
> const { page } = Astro.props
> const { data } = page
> const { \<page_data1\>, \<page_data2\>... } = data
> ```
>
> - SSR no puede usar `getStaticPaths`, pero puede obtener información de otras maneras:
>
> ```astro
> // Aquí definimos el contenido de la página en función del slug.
> const page = [
>   { slug: <slug_1>, title: <title_1>, text: <text_1> },
>   { slug: <slug_1>, title: <title_1>, text: <text_1> },
>   { slug: <slug_1>, title: <title_1>, text: <text_1> },
> ];
> 
> const { slug } = Astro.params;
> 
> // Aquí obtenemos el contenido de la página a partir del slug.
> const page = page.find((page) => page.slug === slug);
> 
> // Si no existe la página, redirigimos a una página 404.
> if (!page) return Astro.redirect("/404");
> const { title, text } = page;
>   ```

[^ref1]: [Curso ASTRO 5 - Midudev](https://www.youtube.com/watch?v=WHqZAXHZN_w)
[^ref2]: [Colecciones de contenido - Astro docs](https://docs.astro.build/es/guides/content-collections/)
[^ref3]: [Rutas estáticas - Astro docs](https://docs.astro.build/es/guides/routing/)
[^ref4]: [Páginas - Astro docs](https://docs.astro.build/es/basics/astro-pages/)
[^ref5]: [Componentes - Astro docs](https://docs.astro.build/es/basics/astro-components/)
[^ref6]: [Plantillas - Astro docs](https://docs.astro.build/es/basics/layouts/)
