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
  <Nav />
  <Contenido />
  <Footer />
</Router>

<style>
  :global(body) {
    margin: 0 auto;
    padding: 0;
    background-color: black;
  }
</style>
