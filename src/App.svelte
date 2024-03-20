<script lang="ts">
  import { onMount } from 'svelte';
  import axios from 'axios';
  
  import bgCoffeeShop from './assets/bg-cafe.jpg';
  import type { CoffeeItem } from './types';
  import CoffeeCard from './lib/CoffeeCard.svelte';

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
              <CoffeeCard {coffeeItem} />
            {/each}
          {:else}
            {#each coffeeListData.filter(coffeeItem => coffeeItem.available) as coffeeItem}
              <CoffeeCard {coffeeItem} />
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
    background-color: $black;
    padding-bottom: 10rem;
    .hero {
      img {
        width: 100%;
        height: 356px;
        object-fit: fill;
      }
    }
  
    .our-collection {
      background-color: $charcoal;
      margin-inline: auto;
      position: relative;
      z-index: 1;
      margin-top: -12rem;
      border-radius: 1rem;
      box-shadow: 0 0 1rem rgba(0, 0, 0, 0.5);
      width: 90%;
      max-width: 60rem;
      color: $cream;
      padding-block: 3rem;
      background-image: url('./assets/vector.svg');
        background-repeat: no-repeat;
        background-position: 70% 1rem;
        background-size: 14rem;

      @include tablet {
        padding: 3rem;
        width: 80%;
      }
      
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
          padding-inline: 1rem;
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

      .coffee-cards {
        @include tablet {
          display: grid;
          grid-template-columns: repeat(auto-fill, minmax(15rem, 1fr));
          row-gap: 2.5rem;
          column-gap: 1rem;
        }
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 2rem;
        margin-top: 2rem;
        list-style: none;
        padding: 0;
      }
    }
  }


</style>
