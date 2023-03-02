<script>
  export let arma = {
    _id: "",
    Nombre: "",
    Tipo: 0,
  };
</script>

<input type="text" id="Nombre" placeholder="Nombre" bind:value={arma.Nombre} />
<input type="text" id="Tipo" placeholder="Tipo" bind:value={arma.Tipo} />
<slot />

<style></style>
