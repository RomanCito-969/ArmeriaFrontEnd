<script>
  export let cliente = {
    nombre: "",
    apellidos: "",
    num_armas: 0,
  };
</script>

<input type="text" placeholder="Nombre" bind:value={cliente.nombre} />
<input type="text" placeholder="Apellidos" bind:value={cliente.apellidos} />
<input
  type="number"
  placeholder="Número de Armas"
  bind:value={cliente.num_armas}
/>
<slot />

<style></style>
