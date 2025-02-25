<script>

    import { onMount } from 'svelte';

    let {dati} = $props();


    async function getDati() {
        const response = await fetch('http://localhost:3080/utente');
        dati = await response.json();
    }

   

    onMount(() => {
        getDati();
        const interval = setInterval(getDati, 5000);
        return () => clearInterval(interval);
    });
</script>

<h1>Tabella Utenti</h1>

{#if dati.length === 0}
    <p>Nessun utente trovato.</p>
{:else}
    <table>
        <thead>
        <tr>
            <th>Nome</th>
            <th>Cognome</th>
            <th>Data di Nascita</th>
            <th>Sesso</th>
            <th>Skills</th>
        </tr>
        </thead>
        <tbody>
        {#each dati as user}
            <tr>
                <td>{user.nome}</td>
                <td>{user.cognome}</td>
                <td>{new Date(user.data).toLocaleDateString()}</td>
                <td>{user.sesso}</td>
                <td>{user.skills.join(', ')}</td>
            </tr>
        {/each}
        </tbody>
    </table>
{/if}

<style>
    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
        background-color: #fff;
        box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
        border-radius: 8px;
        overflow: hidden;
    }

    th, td {
        padding: 12px;
        text-align: left;
        border-bottom: 1px solid #ddd;
    }

    th {
        background-color: #4CAF50;
        color: white;
        font-weight: bold;
    }

    tr:hover {
        background-color: #f1f1f1;
    }

    td {
        color: #333;
    }
</style>