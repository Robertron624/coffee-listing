<script lang="ts">
  import { onMount } from 'svelte';
  import bgCoffeeShop from './assets/bg-cafe.jpg';
  import axios from 'axios';

  import type { CoffeeItem } from './types';

  const coffeeListUrl = 'https://raw.githubusercontent.com/devchallenges-io/web-project-ideas/main/front-end-projects/data/simple-coffee-listing-data.json'

  let coffeeListData: CoffeeItem[] = [];
  let isLoading = true;

  enum Filter {
    All = 'All',
    AvailableNow = 'Available Now',
  }

  let filter: Filter = Filter.All;

  onMount(async () => {
    try {
      const response = await axios.get(coffeeListUrl);
      coffeeListData = response.data;
    } catch (error) {
      console.error(error);
    } finally {
      isLoading = false;
    }
  });
</script>


<main>
  <div class="hero">
    <img
      src={bgCoffeeShop}
    alt="coffee shop paint" />
  </div>
  <section class="our-collection">
    <div class="intro-text">
      <h1>
        Our Collection
      </h1>
      <p>
        Introducing our Coffee Collection, a selection of unique coffees from different roast types and origins, expertly roasted in small batches and shipped fresh weekly.
      </p>
    </div>
    
    {#if isLoading}
      <p>Loading...</p>
    {:else}
      {#if coffeeListData.length > 0}
      <div class="filter-btns">
        <button
          class:active={filter === Filter.All}
          on:click={() => filter = Filter.All}
        >
          All Products
        </button>
        <button
          class:active={filter === Filter.AvailableNow}
          on:click={() => filter = Filter.AvailableNow}
        >
          Available Now
        </button>
      </div>
        <ul class="coffee-cards">
          {#if filter === Filter.All}
            {#each coffeeListData as coffeeItem}
              <li>
                <h2>{coffeeItem.name}</h2>
              </li>
            {/each}
          {:else}
            {#each coffeeListData.filter(coffeeItem => coffeeItem.available) as coffeeItem}
              <li>
                <h2>{coffeeItem.name}</h2>
              </li>
            {/each}
          {/if}
        </ul>
      {:else}
        <p>No data available</p>
      {/if}
    {/if}

  </section>
</main>

<style lang="scss" scoped>

  @import 'vars.scss';

  main {
    position: relative;
    .hero {
      img {
        width: 100%;
        height: auto;
      }
    }
  
    .our-collection {
      background-color: $charcoal;
      padding: 3rem;
      position: absolute;
      top: 100%;
      left: 50%;
      transform: translate(-50%, -50%);
      border-radius: 1rem;
      box-shadow: 0 0 1rem rgba(0, 0, 0, 0.5);
      width: 90%;
      max-width: 50rem;
      color: $cream;

      background-image: url('./assets/vector.svg');
        background-repeat: no-repeat;
        background-position: 70% 1rem;
        background-size: 14rem;
      
      .intro-text {
        display: flex;
        flex-direction: column;


        h1 {
          font-size: 2rem;
          margin-bottom: 1rem;
          margin-top: 0;
        }

        p{
          margin: 0;
          max-width: 27rem;
          font-size: .875rem;
          align-self: center;
          color: $gray;
          text-wrap: pretty;
        }

      }

      .filter-btns {
        margin-top: 2rem;

        button {
          background-color: transparent;
          border: none;
          color: $cream;
          font-size: 1rem;
          padding: .5rem 1rem;
          margin-right: 1rem;
          cursor: pointer;
          transition: all .3s ease-in-out;

          &:hover {
            background-color: $gray;
          }

          &.active {
            background-color: $gray;
          }
        }
      }
    }
  }


</style>
