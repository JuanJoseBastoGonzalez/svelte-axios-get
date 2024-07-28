<script lang="ts">
    import axios from "axios";
    import { onMount } from "svelte";

    let todos: { id: number; title: string; completed: boolean }[] = [];
    let error: string = "";
    let loading: boolean = true;
    let titles: string = "";
    let completed: any;

    onMount(async () => {
        try {
            const response = await axios.get("https://jsonplaceholder.typicode.com/todos");
            // Mapeamos los datos de la API a la estructura esperada
            todos = response.data.map((info: any) => ({
                id: info.id,
                title: info.title,
                completed: info.completed
            }));
        } catch (erro) {
            error = "fail";
        } finally {
            loading = false;
        }
        console.log(todos);
    });
</script>

{#if loading}
    <p>Loading...</p>
{:else if error}
    <p>{error}</p>
{:else}
    <ul>
        {#each todos as todo}
            <li>
                <p>ID: {todo.id}</p>
                <p>Title: {todo.title}</p>
                <p>Completed: {todo.completed ? "Yes" : "No"}</p>
            </li>
        {/each}
    </ul>
{/if}
