---
id: da960a
title: "React en 5 minutos"
imgs: []
created: 2025-06-16T16:23:00Z
modified: 2025-06-24T08:34:43Z
tags: 
  - type/sprout
  - queued/note
alias:
  - "React en 5 minutos"
  - "react en 5 minutos"
---

: [^ref1] [^ref2] [^ref3] [^ref4] [^ref5] [^ref6] [^ref7] [^ref8] [^ref9] [^ref10]

## Introducción a React

> [!NOTE] **React** es una librería de **UI declarativa** escrita en **JavaScript**.

Fué creada por **Facebook (ahora Meta) en 2011**. Se utiliza para desarrollar **aplicaciones web modernas enfocadas a contenido dinámico e interactividad**. Utiliza una **arquitectura basada en componentes** para crear piezas reutilizables y modulares de la interfaz de usuario.

## Características de React

- **Componentes**: **Funciones puras** escritas en **JavaScript** que retornan **HTML**. En React, **todo es un componente**.
- **JSX**: Es una extensión de **JavaScript** que permite agregar marcado similar o igual a **HTML**. Se utiliza por defecto para crear componentes en React, aunque hay otras opciones como **TypeScript**.
- **DOM virtual**: Es una copia del **DOM** que se utiliza para comparar y actualizar únicamente las partes de la aplicación que han cambiado. Esta actualización selectiva hace que los proyectos con React sean muy eficientes y rápidos.
- **One-Way Data Binding**: La información en React sólo viaja en una dirección,para dar un mayor control y comprensión sobre la **propagación de cambios** a lo largo de la aplicación, haciendo también más fácil su depuración y mantenimiento.
- **Hooks**: Piezas de código para dotar de lógica interna a los componentes. Permite a los componentes tener y gestionar estados, compartir información entre ellos o gestionar side-effects, entre otros.

## Conceptos fundamentales

- **Renderizar vs recargar**: Renderizar actualiza la parte del DOM que ha cambiado tras comparar con el DOM virtual. Recargar reinicializa toda la información y estados, y re-renderiza toda la aplicación.
- **Props vs Contexto**: Las props son datos que se pasan de un componente a otro. El contexto es un espacio global que permite el intercambio de información entre componentes.
- **Estado**: Es la "memoria" del componente. Guarda los valores que pueden cambiar con la interacción del usuario.
- **Estados del ciclo de vida de un componente**:
  - **Mounting**: Primer renderizado del componente y su inserción en el DOM.
  - **Updating**: Renderizado del componente cuando las props o estado del componente han cambiado.
  - **Unmounting**: Eliminación del componente del DOM.
- **Hidratación**: Agregación de interactividad y eventos a un componente que ya ha sido renderizado en el servidor.
- **Mutación**: Modificación directa de la información. Se desaconseja en React porque genera errores difíciles de localizar y mal rendimiento.
- **Componente puro**: Es aquel componente que no modifica ningún valor externo a él y que siempre retorna el mismo resultado con las mismas entradas.
- **Lazy Loading**: Patrón de diseño que carga componentes sólo cuando son necesarios. En React además podemos mostrar un placeholder mientras se carga el componente.
- **Side-effects**: Operaciones externas que ocurren como resultado de la ejecución de un componente, como llamadas a APIs, manipulación del DOM o suscripciones a eventos.
- **Propagación de eventos**: Es una técnica que permite a un componente hijo avisar a su padre de que ha sucedido algo, como un clic o un envío de un formulario. Con React esto se implementa pasando las funciones como props.

## Creación de proyectos

> [!NOTE] React se puede usar sólo o con un framework.

React es una **librería** que nos da el control total del proyecto, pero requiere gestionar las dependencias por nuestra cuenta (enrutamiento, datos, etc.).

Utilizada con un **framework** como **NextJS** o **React Router**, obtenemos una estructura predefinida que facilita el desarrollo pero nos limita a las "reglas" del framework.

La elección depende del caso de uso y preferencias personales.

### Creación de proyectos con React

> [!IMPORTANT] Para crear un proyecto con React, lo haremos con Vite, que es un compilador orientado a proyectos web modernos:

```bash
# Vite se utiliza como compilador de React Router.
npm create vite@latest my-react-app --template react
```

> [!NOTE] Proyectos para implementar funcionalidades adicionales:

- **Enrutamiento**:
  - React Router y Tanstack Router
- **Obtención de datos**:
  - React Query, SWR, RTK Query (backend o API REST).
  - Apollo o Relay (GraphQL).

### Creación de proyectos con frameworks

```bash
# Ejemplo con NextJS, un framework de React para desarrollo fullstack.
npx create-next-app@latest my-next-app
```

> [!IMPORTANT] Para instalar React en un proyecto existente:

```bash
npm install react react-dom
```

Después lo importamos en el fichero JavaScript principal:

```js
import { createRoot } from 'react-dom/client';
// Borra el contenido HTML existente
document.body.innerHTML = '<div id="app"></div>';
// Renderiza el componente React en el elemento con id "app"
const root = createRoot(document.getElementById('app'));
root.render(<h1>¡Hola, mundo!</h1>);
```

## Anatomía de un componente

> [!IMPORTANT] Un componente React es una función que retorna un elemento JSX:

```jsx
import React, { useState } from 'react';

// Props del componente
interface TodoListProps {
   userName: string;
   taskList: string[];
   onAddTask: (task: string) => void; // Ejemplo de propagación de eventos
}

/*
* Componente que muestra una lista de tareas y permite agregar nuevas.
* @param taskList 
* @param onAddTask 
*/
export default function TodoList({userName, taskList, onAddTask} : TodoListProps) {

   // Estado del componente
   const [tasks, setTasks] = useState(taskList);

   // Manejador de eventos
   const handleAddTask = () => {
     setTasks([...tasks, 'Nueva tarea']);
     // Llamada a la función onAddTask del padre.
     onAddTask('Nueva tarea');
   };
   
   return (
     <>
       <ul>
         {tasks.map((task, index) => (
           <li key={index}>{task}</li>
         ))}
       </ul>
       <form onSubmit={handleAddTask}>
         <input type="text" />
         <button onClick={handleAddTask}> Agregar tarea </button>
       </form>
     </>
   );
}

// Implementación del componente
export default function App() {

   const insertTask = (task: string) => {
      // Lógica para insertar nuevas tareas en base de datos
   };
  
   return <div>
   <h1>Hola, Usuario!</h1>
   <p> Tareas: </p>
   <TodoList userName={"Usuario"} taskList={[]} onAddTask={(task) => insertTask(task)} />
   </div>;
}
```

[^ref1]: <https://medium.com/@ojebiyifulness/5-main-features-of-react-js-that-developers-must-know-759e222d3699>
[^ref2]: <https://www.elightwalk.com/blog/react-js-key-features-benefits-and-more>
[^ref3]: <https://es.react.dev/learn/writing-markup-with-jsx>
[^ref4]: <https://es.react.dev/learn/writing-markup-with-jsx>
[^ref5]: <https://www.freecodecamp.org/news/full-guide-to-react-hooks/#heading-a-bit-of-history-about-react-and-what-hooks-are-for>
[^ref6]: <https://medium.com/@jhakeshav14275/essential-react-concepts-you-must-know-for-developers-interviews-995bf7e690db>
[^ref7]: <https://medium.com/@rohitdhiman937/react-hydration-explained-5c23349a7c8b>
[^ref8]: <https://es.react.dev/learn/keeping-components-pure>
[^ref9]: <https://www.freecodecamp.org/news/event-propagation-event-bubbling-event-catching-beginners-guide/>
[^ref10]:<https://reactrouter.com/start/modes>
