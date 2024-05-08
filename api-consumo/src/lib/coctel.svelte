<script>
  import { onMount } from 'svelte';
  import Modal from './Modal.svelte';

  let drinks = [];
  let loading = true;
  let error = null;
  let searchTerm = '';
  let selectedDrink = null;

  onMount(async () => {
    fetchDrinks();
  });

  const fetchDrinks = async () => {
    try {
      const response = await fetch('https://www.thecocktaildb.com/api/json/v1/1/filter.php?i=Bourbon');
      const data = await response.json();
      drinks = data.drinks;
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  };

  const filterDrinks = () => {
    if (searchTerm.trim() === '') {
      return drinks;
    }
    return drinks.filter(drink => drink.strDrink.toLowerCase().includes(searchTerm.toLowerCase()));
  };

  const showDrinkDetails = (drink) => {
    selectedDrink = drink;
  };
</script>

<div class="container">
  <h1>Tienda de Cócteles</h1>
  <input type="text" placeholder="Buscar cóctel" bind:value={searchTerm} />

  {#if loading}
    <p>Cargando...</p>
  {:else if error}
    <p>Error: {error}</p>
  {:else}
    <div class="drink-buttons">
      {#each filterDrinks() as drink}
        <button class="drink-button" on:click={() => showDrinkDetails(drink)}>{drink.strDrink}</button>
      {/each}
    </div>
  {/if}
</div>

{#if selectedDrink}
  <Modal drink={selectedDrink} on:close={() => selectedDrink = null} />
{/if}

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #1a1a1a;
    color: #fff;
    padding: 2rem;
  }

  h1 {
    font-size: 3rem;
    margin-bottom: 2rem;
    text-transform: uppercase;
    letter-spacing: 0.2rem;
    text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
  }

  input {
    padding: 0.5rem 1rem;
    font-size: 1.2rem;
    border: none;
    border-radius: 0.5rem;
    background-color: #333;
    color: #fff;
    margin-bottom: 2rem;
    box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
  }

  .drink-buttons {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    grid-gap: 1rem;
  }

  .drink-button {
    background-color: #333;
    color: #fff;
    border: none;
    padding: 1rem;
    border-radius: 0.5rem;
    box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
    font-size: 1.2rem;
    text-transform: uppercase;
    letter-spacing: 0.1rem;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  .drink-button:hover {
    transform: scale(1.05);
    box-shadow: 0 0 20px rgba(255, 255, 255, 0.4);
  }
</style>