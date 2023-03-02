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

<Buscar bind:busqueda={patron} />

<h1 class="text-white">Esta en los Clientes.</h1>

<Cliente bind:cliente={clietInsertar}>
  <Boton tipo="insertar" documento={clietInsertar} coleccion="clientes" />
</Cliente>
<hr />
<div class="container">
  <section class=" row row-cols-1 row-cols-md-3  row-col-lg-4 g-1">
    {#each datosFiltrados as cliente}
      <div class="col">
        <div class="card text-white bg-secondary mb-3 border-info ">
          <Cliente bind:cliente>
            <br />
            <Boton tipo="modificar" documento={cliente} coleccion="clientes" />
            <Boton tipo="eliminar" documento={cliente} coleccion="clientes" />
          </Cliente>
        </div>
      </div>
    {/each}
  </section>
</div>
<hr />

<style>
</style>
