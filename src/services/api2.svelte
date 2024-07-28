<script lang="ts">
    import axios from "axios";
    import { onMount } from "svelte";
let data:any;
let info: any[]=[];
let error:string="";
let loading:boolean;
    onMount(async()=>{
        try{
     const response  = await axios.get("https://jsonplaceholder.typicode.com/photos");
     info = response.data.map((data:any)=>({
id:data.id,
title:data.title
}));
        } catch (erro) {
            error = "fail";
        } finally {
            loading = false;
        }
console.log("logrado");
        
    });
</script>
{#each info as information}
<li>{information.id}{information.title}</li>
    
{/each}