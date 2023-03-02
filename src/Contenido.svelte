<script>
  import { Route } from "svelte-routing";
  import Armeria from "./Armeria.svelte";
  import Clientes from "./Clientes.svelte";
  import Inicio from "./Inicio.svelte";
</script>

<main>
  <Route path="/" component={Inicio} />
  <Route path="/armas" component={Armeria} />
  <Route path="/clientes" component={Clientes} />
</main>

<style>
  main {
    text-align: center;
  }
</style>
