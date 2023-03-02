<script>
    import {Link} from "svelte-routing"
</script>

<header>
    <nav id="menuNavegacion">
        <ul>
            <li><Link class="enlace" to="/">Inicio</Link></li>
            <li><Link class="enlace" to="/armas">Armeria</Link></li>
            <li><Link class="enlace" to="/clientes">Clientes</Link></li>
        </ul>
    </nav>
</header>

<style>
    #menuNavegacion {
        background-color: antiquewhite;
        padding: 5px;
    }
    ul {
        display: flex;
        justify-content: space-around;
        list-style-type: none;
    }
</style>
