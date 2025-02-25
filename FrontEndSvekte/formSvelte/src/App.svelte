<script>
  import Tabella from "./lib/tabella.svelte";

  let nome = $state();
  let cognome = $state();
  let data = $state();
  let sesso = $state();
  let skills = $state();
  let dati = $state([]);

  let responseMessage = $state();

  async function sendData(event) {
    event.preventDefault();
    //alert(`Ciao ${nome}`);
    try {
      const res = await fetch("https://formtpi-backend.vercel.app/utente", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ nome, cognome, data, sesso, skills }),
      });
      if (res.ok) {
        //se la fetch è andata a buon fine
        const data = await res.json();
        console.log(data);
        responseMessage = data.message;
        dati = data.data;
        console.log(responseMessage);
        getDati();
      } else {
        responseMessage = "Errore";
        console.log(responseMessage);
      }
    } catch (error) {
      responseMessage = "Errore di connessione";
      console.log(responseMessage);
    }
  }

  async function getDati() {
    const response = await fetch("https://formtpi-backend.vercel.app/utente");
    dati = await response.json();
  }

 import {onMount} from 'svelte';

  onMount(() => {
    getDati();
</script>

<h1>Benvenuto {nome}</h1>
<form onsubmit={sendData}>
  <p>
    Inserisci il tuo Nome <input
      type="text"
      bind:value={nome}
      placeholder="nome"
    />
  </p>
  <p>
    Inserisci il tuo Cognome <input
      type="text"
      bind:value={cognome}
      placeholder="cognome"
    />
  </p>
  <p>Data di nascita: <input type="date" bind:value={data} /></p>
  <p>Sesso:</p>
  <label
    ><input type="radio" bind:group={sesso} value="Maschio" /> Maschio</label
  >
  <label
    ><input type="radio" bind:group={sesso} value="Femmina" /> Femmina</label
  >
  <label><input type="radio" bind:group={sesso} value="Altro" /> Altro</label>

  <p>Skills:</p>
  <label><input type="checkbox" bind:group={skills} value="HTML" /> HTML</label>
  <label><input type="checkbox" bind:group={skills} value="CSS" /> CSS</label>
  <label
    ><input type="checkbox" bind:group={skills} value="JavaScript" /> JavaScript</label
  >
  <label><input type="checkbox" bind:group={skills} value="PHP" /> PHP</label>

  <div>
    <button type="submit">Invia</button>
  </div>
</form>

<p>{responseMessage}</p>

<Tabella {dati} />


<style>
  form {
    max-width: 400px;
    margin: 20px auto;
    padding: 20px;
    background: #000000;
    border-radius: 8px;
    box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
  }

  p {
    margin-bottom: 10px;
    font-size: 16px;
  }

  input[type="text"],
  input[type="date"] {
    width: 100%;
    padding: 8px;
    margin-top: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 14px;
  }

  label {
    display: block;
    margin-top: 5px;
    font-size: 14px;
  }

  input[type="radio"],
  input[type="checkbox"] {
    margin-right: 5px;
  }

  button {
    width: 100%;
    padding: 10px;
    background-color: #4CAF50;
    color: rgb(0, 0, 0);
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
  }

  button:hover {
    background-color: #45a049;
  }
</style>
