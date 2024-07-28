<script lang="ts">
    import axios from "axios";
    import { onMount } from "svelte";

    let email: { id: number; textos: string }[] = [];
    let loading: boolean = true;
    let error: string | null = null;
    let textos: string = "";
    let name: string = "";
    let userName: string | null = null;

    onMount(async () => {
        await cargarDatos();
    });

    async function cargarDatos() {
        loading = true;
        error = null;
        try {
            const response = await axios.get("https://jsonplaceholder.typicode.com/users");
            // Mapeamos los datos de la API a la estructura esperada
            email = response.data.map((user: any) => ({
                id: user.id,
                textos: user.email,
                name: user.name
            }));
        } catch (err) {
            error = "fail";
        } finally {
            loading = false;
        }
    }

    function entrar() {
        const user = email.find(correo => correo.textos.includes(textos));
        if (user) {
            userName = user.name;
           
        } else {
            userName = null;
        }
      
    }
</script>

<input type="text" bind:value={textos} placeholder="Buscar por email">

{#if loading}
    <p>Loading...</p>
{:else if error}
    <p>{error}</p>
{:else}
    <ul>
        {#each email.filter(correo => correo.textos.includes(textos)) as correo}
            <li>{correo.textos}</li>
        {/each}
    </ul>
    <input type="text" bind:value={name} placeholder="Ingrese su nombre">
    <button on:click={entrar}>Buscar</button>
    {#if userName}
        <p>Nombre del usuario: {userName}</p>
        {#if userName === name}
        <p>{userName}</p>
        {/if}
    {/if}
{/if}
