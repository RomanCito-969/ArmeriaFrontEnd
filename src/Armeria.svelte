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

<h1 class="text-white">Esta en la Armeria</h1>

<Arma bind:arma={armaInsertada}>
  <Boton tipo="insertar" documento={armaInsertada} coleccion="armas" />
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
              <Boton tipo="modificar" documento={arma} coleccion="armas" />
              <Boton tipo="eliminar" documento={arma} coleccion="armas" />
            </div>
          </Arma>
        </div>
      </div>
    {/each}
  </section>
</div>
<hr />

<style>
</style>
