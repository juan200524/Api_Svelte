<script>
    import { createEventDispatcher } from 'svelte';
  
    const dispatch = createEventDispatcher();
    export let drink;
  
    const closeModal = () => {
      dispatch('close');
    };
  </script>
  
  <div class="modal-overlay" on:click={closeModal}>
    <div class="modal" on:click|stopPropagation>
      <button class="close-button" on:click={closeModal}>
        &times;
      </button>
      <div class="modal-content">
        <h2>{drink.strDrink}</h2>
        <img src={drink.strDrinkThumb} alt={drink.strDrink} />
      </div>
    </div>
  </div>
  
  <style>
    .modal-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }
  
    .modal {
      background-color: #1a1a1a;
      color: #fff;
      padding: 2rem;
      border-radius: 0.5rem;
      box-shadow: 0 0 20px rgba(255, 255, 255, 0.4);
      position: relative;
      max-width: 400px;
      text-align: center;
    }
  
    .close-button {
      position: absolute;
      top: 1rem;
      right: 1rem;
      background-color: transparent;
      color: #fff;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
    }
  
    .modal-content h2 {
      font-size: 2rem;
      margin-bottom: 1rem;
      text-transform: uppercase;
      letter-spacing: 0.1rem;
      text-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
    }
  
    .modal-content img {
      max-width: 100%;
      height: auto;
      border-radius: 0.5rem;
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
    }
  </style>