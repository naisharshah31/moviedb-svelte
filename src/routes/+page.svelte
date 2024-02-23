<script>
    import { onMount } from 'svelte';

    let responseData = '';

    onMount(async () => {
        try {
            responseData = await load();
        } catch (error) {
            responseData = 'Error fetching data';
        }
    });

    async function load() {
        const res = await fetch('https://api.themoviedb.org/3/movie/popular?language=en-US&page=1&api_key=7a97c8bd3c7ee651f5f2866e7cf25411')
        const data = await res.json();
        if (res.ok) {
            return data.results;
        }
        return {
            status: res.status,
            error: new Error()
        };
    }
</script>


<h1>Data from API:</h1>

{#await responseData}
    <p>...waiting</p>
{:then responseData}
    {#each responseData as movie}
        <p>The number is {movie.original_title}</p>
    {/each}
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}