# Armeria FrontEnd con Svelte

## Inicio de proyecto de Svelte

Para iniciar un proyecto de svelte, ejecutamos:

```console
npx  degit  sveltejs/template   ArmeriaFrontEnd
```

---

## Nos movemos al proyecto creado

```console
├── package.json
├── public
│   ├── favicon.png
│   ├── global.css
│   └── index.html
├── README.md
├── rollup.config.js
└── src
    ├── App.svelte
    └── main.js
```

El archivo `package.json` es el archivo de gestión de proyecto y dependencias. En él. podremos editar el nombre del autor, la versión, el tipo de licencia, etc.

La carpeta `public` contiene el frontend en forma de contenido estático, el cual deberemos subir a nuestro servidor de producción una vez finalizado el proyecto.

El archivo `README.md` editado a nuestro gusto.

El archivo `rollup.config.js` contiene la configuración del empaquetador, que en este caso es **Rollup**. Otros frameworks utilizan otros empaquetadores como Webpack o Parcel. No debemos borrar este archivo. Tampoco lo editaremos, por ahora.

Finalmente, la carpeta `src` va a contener **nuestro código y todos los componentes web que vayamos creando**. Cada vez que realicemos un cambio en los archivos de dicha carpeta, rollup volverá a compilar y pondrá el resultado en `public/build/bundle.css` y `public/build/bundle.js`.

El archivo `public/index.html` tiene enlaces a los anteriores.

---

## Empezar a trabajar en el proyecto

Los archivos que crearemos y modificaremos seran los siguientes:

En `scr/`:

**Modificados:**

```markdown
    * App.svelte
    * main.js
```

**Creados:**

```markdown
    * Arma.svelte
    * Armeria.svelte
    * Boton.svelte
    * Buscar.svelte
    * Cliente.svelte
    * Clientes.svelte
    * Contenido.svelte
    * Footer.svelte
    * Inicio.svelte
    * Nav.svelte
    * store.js
```

---

### App.svelte

```html
<script>
  import { setContext } from "svelte";

  import Nav from "./Nav.svelte";
  import Contenido from "./Contenido.svelte";
  import { Router } from "svelte-routing";
  import Footer from "./Footer.svelte";

  const URL = {
    armas: "https://armeria.fly.dev/api/armas/",
    clientes: "https://armeria.fly.dev/api/clientes/",
  };

  setContext("URL", URL);
</script>

<Router>
  <nav />
  <Contenido />
  <footer />
</Router>

<style>
  :global(body) {
    margin: 0 auto;
    padding: 0;
    background-color: black;
  }
</style>
```

> **URL** esta constante es donde escribiremos las urls a nuestro `BACKEND`.

### main.js

```javascript
import App from "./App.svelte";

const app = new App({
  target: document.body,
  props: {
    name: "armeria",
  },
});

export default app;
```

---

### Arma.svelte

En este archivo generamos el modelo de dato de Arma.

- Arma
  - nombre : `String`
  - tipo : `String`

```html
<script>
  export let arma = {
    nombre: "",
    tipo: "",
  };
</script>

<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Nombre"
  bind:value="{arma.nombre}"
/>
<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Tipo"
  bind:value="{arma.tipo}"
/>
<slot />

<style></style>
```

### Armeria.svelte

En **Armeria.svelte** hacemos la llamada asincrona para obtener las armas de la base de datos y almacenarlos en un variable, la cual mostraremos con un for each.

Importamos los componentes de **Burcar, Arma, Boton y store.js**.

```html
<script>
  import { getContext } from "svelte";
  import { data } from "./store.js";
  import { onMount } from "svelte";
  import Buscar from "./Buscar.svelte";
  import Arma from "./Arma.svelte";
  import Boton from "./Boton.svelte";

  const URL = getContext("URL");

  let armaInsertada = {};
  let datosFiltrados = [];
  let patron;
  let getArmas = async () => {
    const response = await fetch(URL.armas);
    $data = await response.json();
  };

  onMount(getArmas);

  $: datosFiltrados = $data.filter((arma) =>
    RegExp(patron, "i").test(arma.nombre)
  );
</script>

<Buscar bind:busqueda="{patron}" />

<h1 class="text-white">Esta en la Armeria</h1>

<Arma bind:arma="{armaInsertada}">
  <Boton tipo="insertar" documento="{armaInsertada}" coleccion="armas" />
</Arma>
<hr />
<div class="container">
  <section class=" row row-cols-1 row-cols-md-3  row-col-lg-4 g-2">
    {#each datosFiltrados as arma}
    <div class="col">
      <div class="card text-white bg-secondary mb-3 border-info ">
        <Arma bind:arma>
          <br />
          <div class="container-fluid " role="group">
            <Boton tipo="modificar" documento="{arma}" coleccion="armas" />
            <Boton tipo="eliminar" documento="{arma}" coleccion="armas" />
          </div>
        </Arma>
      </div>
    </div>
    {/each}
  </section>
</div>
<hr />

<style></style>
```

### Boton.svelte

Es un componente donde hacemos parte los metodos del **CRUD**.

- **CREATE** el metodo insertar().
- **UPDATE** el metodo modificar().
- **DELETE** el metodo eliminar().

Y importa el archivo **store.js** que usamos como almacen de los datos de la llamada de lectura.

```html
<script>
  import { onMount } from "svelte";
  import { getContext } from "svelte";
  import { data } from "./store";

  export let tipo = "insertar";
  export let coleccion = "arma";
  export let documento = {};
  let url;

  let URL = getContext("URL");
  let handler = () => {};

  function insertar() {
    let opciones = {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(documento),
    };
    console.log(documento);
    fetch(url, opciones)
      .then((res) => res.json())
      .then((dato) => ($data = [...$data, dato]))
      .catch((error) => console.log(error));
  }
  function modificar() {
    let opciones = {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(documento),
    };
    fetch(url + documento._id, opciones)
      .then((res) => res.json())
      .then((dato) => console.log(dato))
      .catch((error) => console.log(error));
  }
  function eliminar() {
    let opciones = {
      method: "DELETE",
    };
    fetch(url + documento._id, opciones)
      .then((res) => res.json())
      .then((dato) => ($data = $data.filter((doc) => doc._id != dato._id)))
      .catch((error) => console.log(error));
  }

  function setUp() {
    switch (tipo) {
      case "insertar":
        handler = insertar;
        break;
      case "modificar":
        handler = modificar;
        break;
      case "eliminar":
        handler = eliminar;
        break;
      default:
        break;
    }
    switch (coleccion) {
      case "armas":
        url = URL.armas;
        break;
      case "clientes":
        url = URL.clientes;
        break;
      default:
    }
  }
  onMount(setUp);
</script>

<input
  type="button"
  class="bg-transparent text-white"
  value="{tipo.toLocaleUpperCase()}"
  on:click="{handler}"
/>
```

### Buscar.svelte

Este componente hace un input tipo="search" que grindea su valor a la variable `busqueda`.

```html
<script>
  export let busqueda = "";
</script>

<input type="search" id="Buscador" bind:value="{busqueda}" />
```

### Cliente.svelte

En este archivo generamos el modelo de dato de Cliente.

- Cliente
  - nombre : `String`
  - apellido : `String`
  - num_armas : `Number`

```html
<script>
  export let cliente = {
    nombre: "",
    apellidos: "",
    num_armas: 0,
  };
</script>

<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Nombre"
  bind:value="{cliente.nombre}"
/>
<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Apellidos"
  bind:value="{cliente.apellidos}"
/>
<input
  type="number"
  class="bg-transparent text-white"
  placeholder="Número de Armas"
  bind:value="{cliente.num_armas}"
/>
<slot />

<style></style>
```

### Clientes.svelte

En **CLiente.svelte** hacemos la llamada asincrona para obtener las armas de la base de datos y almacenarlos en un variable, la cual mostraremos con un for each.

Importamos los componentes de **Burcar, Cliente, Boton y store.js**.

```html
<script>
  import { getContext } from "svelte";
  import { data } from "./store.js";
  import { onMount } from "svelte";
  import Buscar from "./Buscar.svelte";
  import Cliente from "./Cliente.svelte";
  import Boton from "./Boton.svelte";

  const URL = getContext("URL");

  let clietInsertar = {};
  let datosFiltrados = [];
  let patron;
  let getClientes = async () => {
    const response = await fetch(URL.clientes);
    $data = await response.json();
  };

  onMount(getClientes);

  $: datosFiltrados = $data.filter((cliente) =>
    RegExp(patron, "i").test(cliente.nombre)
  );
</script>

<Buscar bind:busqueda="{patron}" />

<h1 class="text-white">Esta en los Clientes.</h1>

<Cliente bind:cliente="{clietInsertar}">
  <Boton tipo="insertar" documento="{clietInsertar}" coleccion="clientes" />
</Cliente>
<hr />
<div class="container">
  <section class=" row row-cols-1 row-cols-md-3  row-col-lg-4 g-1">
    {#each datosFiltrados as cliente}
    <div class="col">
      <div class="card text-white bg-secondary mb-3 border-info ">
        <Cliente bind:cliente>
          <br />
          <Boton tipo="modificar" documento="{cliente}" coleccion="clientes" />
          <Boton tipo="eliminar" documento="{cliente}" coleccion="clientes" />
        </Cliente>
      </div>
    </div>
    {/each}
  </section>
</div>
<hr />

<style></style>
```

### Contenido.svelte

**Contenido.svelte** tiene el enrutador para ir a los contenidos de **Inicio-Armeria-Clientes**.

```html
<script>
  import { Route } from "svelte-routing";
  import Armeria from "./Armeria.svelte";
  import Clientes from "./Clientes.svelte";
  import Inicio from "./Inicio.svelte";
</script>

<main>
  <Route path="/" component="{Inicio}" />
  <Route path="/armas" component="{Armeria}" />
  <Route path="/clientes" component="{Clientes}" />
</main>

<style>
  main {
    text-align: center;
  }
</style>
```

### Footer.svelte

Es un footer general para la pagina que contiene mis redes.

```html
<script>
  import { Link } from "svelte-routing";
</script>

<footer>
  <section class="d-flex justify-content-center">
    <p class="text-white">Román Solanas Barrera</p>
    <a
      href="https://github.com/RomanCito-969"
      target="_blank"
      rel="noopener noreferrer"
      class="link-light mx-3"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
        fill="currentColor"
        class="bi bi-github"
        viewBox="0 0 16 16"
      >
        <path
          d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.012 8.012 0 0 0 16 8c0-4.42-3.58-8-8-8z"
        />
      </svg>
    </a>
    <a
      href="https://www.linkedin.com/in/romansolanasbarrera969/"
      target="_blank"
      rel="noopener noreferrer"
      class="link-light mx-3"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="16"
        height="16"
        fill="currentColor"
        class="bi bi-linkedin"
        viewBox="0 0 16 16"
      >
        <path
          d="M0 1.146C0 .513.526 0 1.175 0h13.65C15.474 0 16 .513 16 1.146v13.708c0 .633-.526 1.146-1.175 1.146H1.175C.526 16 0 15.487 0 14.854V1.146zm4.943 12.248V6.169H2.542v7.225h2.401zm-1.2-8.212c.837 0 1.358-.554 1.358-1.248-.015-.709-.52-1.248-1.342-1.248-.822 0-1.359.54-1.359 1.248 0 .694.521 1.248 1.327 1.248h.016zm4.908 8.212V9.359c0-.216.016-.432.08-.586.173-.431.568-.878 1.232-.878.869 0 1.216.662 1.216 1.634v3.865h2.401V9.25c0-2.22-1.184-3.252-2.764-3.252-1.274 0-1.845.7-2.165 1.193v.025h-.016a5.54 5.54 0 0 1 .016-.025V6.169h-2.4c.03.678 0 7.225 0 7.225h2.4z"
        /></svg
    ></a>
  </section>
</footer>
```

### Inicio.svelte

Contiene la pagina de inicio de la aplicación creada.

```html
<script></script>

<main>
  <section class="text-aling-center my-5 py-5">
    <h1 class="text-secondary">Armeria</h1>
    <h1 class="text-secondary">FrontEnd</h1>
  </section>
</main>

<style></style>
```

### Nav.svelte

Es la `Barra de navegación` de la pagina. La cual contiene los Link de navegación para ir de pagina a pagina.

```html
<script>
  import { Link } from "svelte-routing";
</script>

<header>
  <nav class="navbar navbar-expand-lg  navbar-dark bg-dark">
    <div class="container-fluid">
      <Link class="navbar-brand" to="/">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-house"
          viewBox="0 0 16 16"
        >
          <path
            d="M8.707 1.5a1 1 0 0 0-1.414 0L.646 8.146a.5.5 0 0 0 .708.708L2 8.207V13.5A1.5 1.5 0 0 0 3.5 15h9a1.5 1.5 0 0 0 1.5-1.5V8.207l.646.647a.5.5 0 0 0 .708-.708L13 5.793V2.5a.5.5 0 0 0-.5-.5h-1a.5.5 0 0 0-.5.5v1.293L8.707 1.5ZM13 7.207V13.5a.5.5 0 0 1-.5.5h-9a.5.5 0 0 1-.5-.5V7.207l5-5 5 5Z"
          />
        </svg>
      </Link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon" />
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <Link class="nav-link active" to="/">Inicio</Link>
          </li>
          <li class="nav-item">
            <Link class="nav-link" to="/armas">Armeria</Link>
          </li>
          <li class="nav-item">
            <Link class="nav-link" to="/clientes">Clientes</Link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</header>

<style>
</style>
```

### store.js

Es un archivo escribible donde almacenamos los datos de la operacion asincrona de recuperación de datos.

```javascript
import { writable } from "svelte/store";

export const data = writable([]);
```

## Ejecutamos la aplicación

Para ejecutar la aplicación deberemos ejecutar:

```console
npm  run  dev
```

La ejecución del script anterior dará error. El motivo es que no hemos instalado las dependencias que aparecen indicadas en el archivo `package.json`.

Para instalar dichas dependencias, ejecutamos:

```console
npm  install
```

Dicho comando, leerá el archivo `package.json`, e instalará todas las dependencias que aparecen ahí. Ahora ya podemos volver a ejecutar `npm  run  dev`.

Podrás ver la aplicación en [localhost:5000](http://localhost:5000).

## El despiege web

Realizado en [Vercel](https://vercel.com/) en [ArmeriaFrontEnd](https://armeria-frontend-three.vercel.app/)

para aprender el despliege siga la documentación de este enlace : [Documentación](https://github.com/jamj2000/tiendafrontend/blob/master/README.md)
