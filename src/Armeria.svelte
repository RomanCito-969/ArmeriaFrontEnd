<script>
  import { getContext } from "svelte";
  import { data } from "./store.js";
  import { onMount } from "svelte";
  import Buscar from "./Buscar.svelte";
  import Arma from "./Arma.svelte";
  import Boton from "./Boton.svelte";

  const URL = getContext("URL");

  let armasInsertar = {};
  let datosFiltrados = [];
  let patron;
  let getArmas = async () => {
    const response = await fetch(URL.armas);
    $data = await response.json();
  };

  onMount(getArmas);

  $: datosFiltrados = $data.filter((armas) =>
    RegExp(patron, "i").test(armas.nombre)
  );
</script>

<Buscar bind:busqueda={patron} />

<h1>Esta en la Armeria</h1>

<Arma bind:arma={armasInsertar}>
  <Boton tipo="insertar" documento={armasInsertar} />
</Arma>
<hr />
<section class="listaArmas">
  {#each datosFiltrados as arma}
    <div>
      <Arma bind:arma>
        <br />
        <Boton tipo="modificar" documento={arma} />
        <Boton tipo="eliminar" documento={arma} />
      </Arma>
    </div>
  {/each}
</section>
<hr />

<style>
  .listaArmas {
    display: grid;
    grid-template-columns: auto auto auto auto;
    grid-gap: 5px;
  }
</style>
