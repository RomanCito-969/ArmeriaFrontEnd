<script>
  export let cliente = {
    Nombre: "",
    Apellidos: "",
    Num_Armas: 0,
  };
</script>

<input
  type="text"
  id="Nombre"
  placeholder="Nombre"
  bind:value={cliente.Nombre}
/>
<input
  type="text"
  id="Apellidos"
  placeholder="Apellidos"
  bind:value={cliente.Apellidos}
/>
<input
  type="Number"
  id="Num_Armas"
  placeholder="Número de Armas"
  bind:value={cliente.Num_Armas}
/>
<slot />

<style></style>
