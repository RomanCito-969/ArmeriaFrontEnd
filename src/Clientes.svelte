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

<h1>Esta en los Clientes.</h1>

<Cliente bind:cliente={clietInsertar}>
  <Boton tipo="insertar" documento={clietInsertar} />
</Cliente>
<hr />
<section class="listaAticulos">
  {#each datosFiltrados as cliente}
    <div>
      <Cliente bind:cliente>
        <br />
        <Boton tipo="modificar" documento={cliente} />
        <Boton tipo="eliminar" documento={cliente} />
      </Cliente>
    </div>
  {/each}
</section>
<hr />

<style>
</style>
