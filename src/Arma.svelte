<script>
  export let arma = {
    nombre: "",
    tipo: "",
  };
</script>

<input type="text" placeholder="Nombre" bind:value={arma.nombre} />
<input type="text" placeholder="Tipo" bind:value={arma.tipo} />
<slot />

<style></style>
