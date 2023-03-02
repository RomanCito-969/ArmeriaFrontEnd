<script>
  import { onMount } from "svelte";
  import { getContext } from "svelte";
  import { data } from "./store";

  export let tipo = "insertar";
  export let documento = {};

  let URL = getContext("URL");
  let handler = () => {};

  function insertar() {
    console.log("Insertar");
    let opciones = {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(documento),
    };
    fetch(URL.armas, opciones)
      .then((res) => res.json())
      .then((dato) => ($data = [...$data, dato]))
      .catch((error) => console.log(error));
  }
  function modificar() {
    console.log("Modificar");
    let opciones = {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(documento),
    };
    fetch(URL.armas + documento._id, opciones)
      .then((res) => res.json())
      .then((dato) => console.log(dato))
      .catch((error) => console.log(error));
  }
  function eliminar() {
    console.log("Eliminar");
    let opciones = {
      method: "DELETE",
    };
    fetch(URL.armas + documento._id, opciones)
      .then((res) => res.json())
      .then((dato) => ($data = $data.filter((doc) => doc._id != dato._id)))
      .catch((error) => console.log(error));
  }

  function setUp() {
    switch (tipo) {
      case "insertar":
        handler = insertar;
        break;
      case "modificar":
        handler = modificar;
        break;
      case "eliminar":
        handler = eliminar;
        break;
      default:
        break;
    }
  }
  onMount(setUp);
</script>

<input type="button" value={tipo.toLocaleUpperCase()} on:click={handler} />
