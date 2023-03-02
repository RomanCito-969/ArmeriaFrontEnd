<script>
  import { onMount } from "svelte";
  import { getContext } from "svelte";
  import { data } from "./store";

  export let tipo = "insertar";
  export let coleccion = "arma";
  export let documento = {};
  let url;

  let URL = getContext("URL");
  let handler = () => {};

  function insertar() {
    let opciones = {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(documento),
    };
    console.log(documento);
    fetch(url, opciones)
      .then((res) => res.json())
      .then((dato) => ($data = [...$data, dato]))
      .catch((error) => console.log(error));
  }
  function modificar() {
    let opciones = {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(documento),
    };
    fetch(url + documento._id, opciones)
      .then((res) => res.json())
      .then((dato) => console.log(dato))
      .catch((error) => console.log(error));
  }
  function eliminar() {
    let opciones = {
      method: "DELETE",
    };
    fetch(url + documento._id, opciones)
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
    switch (coleccion) {
      case "armas":
        url = URL.armas;
        break;
      case "clientes":
        url = URL.clientes;
        break;
      default:
    }
  }
  onMount(setUp);
</script>

<input type="button" value={tipo.toLocaleUpperCase()} on:click={handler} />
