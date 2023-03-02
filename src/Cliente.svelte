<script>
  export let cliente = {
    nombre: "",
    apellidos: "",
    num_armas: 0,
  };
</script>

<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Nombre"
  bind:value={cliente.nombre}
/>
<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Apellidos"
  bind:value={cliente.apellidos}
/>
<input
  type="number"
  class="bg-transparent text-white"
  placeholder="Número de Armas"
  bind:value={cliente.num_armas}
/>
<slot />

<style></style>
