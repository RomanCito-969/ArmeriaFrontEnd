<script>
  export let arma = {
    nombre: "",
    tipo: "",
  };
</script>

<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Nombre"
  bind:value={arma.nombre}
/>
<input
  type="text"
  class="bg-transparent text-white"
  placeholder="Tipo"
  bind:value={arma.tipo}
/>
<slot />

<style></style>
