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

<Buscar bind:busqueda={patron} />

<h1>Esta en la Armeria</h1>

<Arma bind:arma={armaInsertada}>
  <Boton tipo="insertar" documento={armaInsertada} coleccion="armas" />
</Arma>
<hr />
<section class="listaArmas">
  {#each datosFiltrados as arma}
    <div>
      <Arma bind:arma>
        <br />
        <Boton tipo="modificar" documento={arma} coleccion="armas" />
        <Boton tipo="eliminar" documento={arma} coleccion="armas" />
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
